---
layout: post
title: SQLAlchemy Bulk Insert & Update 알아보기
---

오늘은 PostgreSQL 데이터베이스에 SQLAlchemy Bulk Insert & Update 작업을 해보려고 합니다.

PostgreSQL 데이터베이스는 지난 번과 마찬가지로 로컬에 도커 컨테이너를 따로 띄어놓고 작업해보겠습니다.

# 개요

다량의 데이터를 추가/갱신할 때 레거시 코드처럼 Session.add() 메소드는 적절하지 못해보여서 벌크 작업으로 알아보았습니다.

이 상황에서 사용할 수 있는 메소드는 add_all, bulk_save_objects, bulk_insert_mappings / bulk_update_mappings 정도로 나열할 수 있습니다.

또한 SQLAlchemy Core도 포함할 수 있습니다.

## PostgreSQL docker-compose 구성

작업하기 전, PostgreSQL 컨테이너로 실행해서 테스트할 데이터베이스를 준비합니다.

```yml
version: '3.8'
services:
  postgres:
    image: postgres:14.1
    restart: always
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

postgres 이미지로 시작하며, 볼륨과 init-user-db.sh 파일을 지정했습니다.

```bash
#!/bin/bash
set -e

psql -v ON_ERROR_STOP=1 --username "$POSTGRES_USER" --dbname "$POSTGRES_DB" <<-EOSQL
    CREATE TABLE article (
    id SERIAL NOT NULL,
    description VARCHAR(256) NOT NULL,
    CONSTRAINT id_pk PRIMARY KEY (id)
);
EOSQL
```

```sh
chmod +x init-user-db.sh
```

위 init-user-db.sh 스크립트로 테이블을 미리 만들고 스크립트로 실행 가능하도록 변경합니다.

## 패키지 준비

```
SQLAlchemy==1.4.29
psycopg2-binary==2.9.3
```

PostgreSQL 데이터베이스를 조작할 예정이므로 SQLAlchemy, psycopg2-binary 패키지를 준비합니다.

## 코딩

* 엔진

```python
@contextlib.contextmanager
def session_manager():
    engine = create_engine(DB_URL, echo=True)
    session = Session(bind=engine)
    Base.metadata.create_all(engine)
    yield session
    session.close()
```

내장되어있는 contextlib 라이브러리로 컨텍스트 매니저를 만들어 세션에 사용합니다. 

세션을 사용하고 닫습니다.

* 테이블

```python
class Article(Base):
    __tablename__ = "article"
    id = Column(Integer, primary_key=True)
    description = Column(String(255))
```

스펙대로 테이블 클래스를 만들어서 준비합니다.

* add_all

```python
with session_manager() as session:
    session.add_all(
        [Article(description="test 1"), Article(description="test 2"), Article(description="test 3")]
    )
    session.commit()
```

먼저 add_all 구현은 add를 반복 호출하게 랩핑되었으므로 실제 동작은 아래와 같이 구성됩니다.

```python
def add_all(self, instances):
    for instance in [Article(description="test 1"), Article(description="test 2"), Article(description="test 3")]:
      self.add(instance, _warn=False)
    self.commit()
```

그러므로 실제 벌크 동작보다 여러 객체를 편리하게 넣는다는 점에 초점을 맞출 수 있습니다.

* bulk_save_objects

```python
with session_manager() as session:
    session.bulk_save_objects(
        [Article(description="test 1"), Article(description="test 2"), Article(description="test 3")],
        return_defaults=return_defaults
    )
    session.commit()
```

bulk_save_objects는 이름처럼 객체들을 벌크 작업으로 저장하는 메소드로서, executemany 동작을 수행하게 됩니다.

```python
if return_defaults and isstates:
    identity_cls = mapper._identity_class
    identity_props = [p.key for p in mapper._identity_key_props]
    for state, dict_ in states:
        state.key = (
            identity_cls,
            tuple([dict_[key] for key in identity_props]),
        )
```

return_defaults 파라미터로 ID 값을 포함해서 반환할 수 있지만, 위 코드처럼 내부 구현을 보면 속도에서 불리하다는 점을 알 수 있습니다.

```python
for i in save_objs:
  i.description = i.description.upper()

session.bulk_save_objects(save_objs)
session.commit()
```

가져온 객체를 조작해서 bulk_save_objects 동일하게 수행해서 다시 업데이트할 수 있습니다.

* bulk_insert_mappings / bulk_update_mappings

```python
with session_manager() as session:
    session.bulk_insert_mappings(
        Article, [dict(description="test 1"), dict(description="test 2"), dict(description="test 3")],
        return_defaults=return_defaults
    )
    session.commit()

    session.bulk_update_mappings(
        Article, [dict(id=118, description="TEST 1"), dict(id=119, description="TEST 2")]
    )
    session.commit()
```

bulk_insert_mappings / bulk_update_mappings는 객체 리스트가 아닌 dictionary 리스트와 맵핑할 테이블 모델로 executemany 동작을 수행합니다.

새롭게 추가할 떄는 bulk_insert_mappings, 업데이트할 때는 동일 ROW 파악하기 위해서 PK 값도 명시하고 bulk_update_mappings를 사용합니다.

bulk_save_objects 메소드보다 객체를 만드는 순간 오버헤드를 줄일 수 있다고 합니다.

```python
if return_defaults and isstates:
    identity_cls = mapper._identity_class
    identity_props = [p.key for p in mapper._identity_key_props]
    for state, dict_ in states:
        state.key = (
            identity_cls,
            tuple([dict_[key] for key in identity_props]),
        )
```

bulk_save_objects처럼 return_defaults 파라미터로 ID 값을 포함해서 반환할 수 있지만, 위 코드처럼 내부 구현을 보면 속도에서 불리하다는 점을 알 수 있습니다.

* core

```python
with session_manager() as session:
    with session.bind.begin() as conn:
        conn.execute(
            insert(Article.__table__),
            [dict(description="test 1"), dict(description="test 2"), dict(description="test 3")]
        )

        conn.execute(update(Article.__table__).where(Article.id == bindparam('id')).values(
                {'id': bindparam('id'), 'description': bindparam('description')}
            ), [dict(id=1, description="TEST 1"), dict(id=2, description="TEST 2")])
```

SQLAlchemy Core도 벌크 insert 또는 update 동작으로 진행할 수 있습니다.

ORM은 기본적으로 고성능을 위한 것이 아니므로 core insert, update를 세션에서 받아서 처리합니다.

## 퍼포먼스

https://docs.sqlalchemy.org/en/14/faq/performance.html#i-m-inserting-400-000-rows-with-the-orm-and-it-s-really-slow

해당 문서에 따르면 core 로직이 가장 빠르고, bulk_insert_mappings, bulk_save_objects, ORM 순서로 느려지는 것을 볼 수 있습니다.

이처럼 같은 결과를 보여줘도 요구하는 데이터 타입과 처리 방식이 다르므로, 구현하고 싶은 로직에 따라 차이점을 파악해서 고려할 필요가 있다고 생각합니다.

감사합니다.
