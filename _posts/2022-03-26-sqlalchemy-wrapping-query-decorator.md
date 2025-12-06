---
layout: post
title: SQLAlchemy Python decorator 쿼리 랩핑하기
---

오늘은 SQLAlchemy compiles 데코레이터로 SQL 쿼리를 커스텀해서 사용해보려고 합니다.

PostgreSQL 데이터베이스를 기준으로 작성했으며 도커 컨테이너를 따로 띄어놓고 작업해보겠습니다.

# 개요

SQLAlchemy bulk_save_objects로 ON CONFLICT DO NOTHING 쿼리도 포함했으면 좋겠다는 생각에 찾아보았고, compiles 데코레이터를 알게되었습니다.

그래서 기존 select, insert 동작이 어떤 쿼리로 동작하는지 먼저 알아보고, compiles 데코레이터를 적용한 뒤에 원하는 쿼리로 동작하는지 확인해보겠습니다.

## PostgreSQL 구성

실습을 시작하기 전에 테스트할 데이터베이스를 준비합니다.

```yml
version: '3.8'
services:
  postgres:
    image: postgres:14.2
    environment:
      POSTGRES_PASSWORD: password
      POSTGRES_USER: admin
      POSTGRES_DB: content
      POSTGRES_INITDB_ARGS: --encoding=UTF-8
    volumes:
    - ./pg_data:/var/lib/postgresql/data
    - ./init-user-db.sh:/docker-entrypoint-initdb.d/init-user-db.sh
    ports:
    - "5432:5432"
```

```bash
#!/bin/bash
set -e
psql -v ON_ERROR_STOP=1 --username "$POSTGRES_USER" --dbname "$POSTGRES_DB" <<-EOSQL
    CREATE TABLE article (
    id SERIAL NOT NULL PRIMARY KEY,
    description VARCHAR(256) NOT NULL
);
EOSQL
```

```sh
chmod +x init-user-db.sh
```

postgres 이미지를 기반해서 지정한 스크립트로 테이블을 만듭니다.

## 실습

```
SQLAlchemy==1.4.32
psycopg2-binary==2.9.3
```

SQLAlchemy, psycopg2-binary 패키지를 준비합니다.

```python
Base = declarative_base()


class Article(Base):
    __tablename__ = "article"
    id = Column(Integer, primary_key=True)
    description = Column(String(255))
```

클래스를 만들고 데이터베이스와 맵핑합니다.

```python
select_query = select(Article).where(Article.description == 'test')
query_string = select_query.compile(dialect=postgresql.dialect())
query_string = select_query.compile(dialect=postgresql.dialect(), compile_kwargs={"literal_binds": True})
```

우선 간단하게 select 쿼리를 직접 문자열로 봅니다.

literal_binds True 값으로 전달하면 %(description_1)s 부분에 바운드 파라미터 문자열을 합치면서 아래와 같이 SQL 쿼리가 나오게 됩니다.

SELECT article.id, article.description FROM article WHERE article.description = 'test'

```python
@compiles(Insert, "postgresql")
def compiles_function(element, compiler, **kw):
    query = compiler.visit_insert(element, **kw)
    return query + " ON CONFLICT DO NOTHING"
```

insert 동작을 가져오기 위해서 visit_insert 호출하고, 해당 쿼리 마지막에 ON CONFLICT DO NOTHING 문자열을 붙였습니다.

compiles 데코레이터로 인하여 모든 postgresql의 Insert 동작이 바뀌어서 작동합니다.

```python
insert_query = insert(Article).values(id=1, description='test')
query_string = insert_query.compile(dialect=postgresql.dialect())
query_string = insert_query.compile(dialect=postgresql.dialect(), compile_kwargs={"literal_binds": True})
```

다시 insert 동작을 수행해보면, 아래와 같이 변경되어 출력됩니다.

INSERT INTO article (id, description) VALUES (1, 'username') ON CONFLICT DO NOTHING

이렇게 쿼리에 적용되는 것을 확인했으니 처음에 궁금했던 bulk_save_objects도 실행해봅니다.

```python
engine = create_engine(DB_URL, echo=True)
session = Session(bind=engine)
session.bulk_save_objects([Article(id=1, description='test_1'), 
                           Article(id=2, description='test_2')])
session.commit()
```

DB 엔진으로 세션과 연결하고, bulk_save_objects 수행하면 echo 파라미터로 인하여 쿼리가 보이게 됩니다.

정상적으로 INSERT - ON CONFLICT DO NOTHING 동작해서 새로운 row만 'test_2'로 추가되고, 기존 row는 그대로 유지하게 됩니다.

```python
from sqlalchemy.ext.compiler import deregister
deregister(Insert)
```

deregister 함수를 호출하면 등록한 커스텀 동작이 해제되어 기본 동작으로 수행됩니다.
