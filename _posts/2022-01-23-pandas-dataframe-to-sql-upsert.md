---
layout: post
title: Pandas to_sql 메소드로 PostgreSQL Upsert 구현하기
---

오늘은 Pandas dataframe 데이터로 PostgreSQL 데이터베이스에 Upsert 작업을 해보려고 합니다.

PostgreSQL 데이터베이스는 로컬에 따로 띄어놓고 작업해보겠습니다.

# 개요

(현 시점 22년 1월 23일 기준으로) Pandas에서 제공하는 to_sql 메소드에서는 각 행마다 데이터베이스의 PK나 유니크 제약조건이 충돌날 때 업데이트하는 로직을 바로 사용할 수 없다고 알고있습니다.

16년부터 Upsert 관련 이슈와 PR은 꾸준히 이어지고 있는 것으로 보여집니다.

https://github.com/pandas-dev/pandas/issues/14553

https://github.com/pandas-dev/pandas/pull/29636

만약 해당 PR이 통과되면, on_conflict 파라미터로 'do_nothing', 'do_update' 옵션을 사용할 수 있겠지만... 일단 당장은 직접 구현하기 위해 찾아보았습니다.

그래서 Pandas의 to_sql 메소드를 좀 더 찾아보니, bulk 옵션 외로 method 파라미터에 함수를 넣을 수 있었습니다.

```
method{None, ‘multi’, callable}, optional

Controls the SQL insertion clause used:
  None : Uses standard SQL INSERT clause (one per row).
  ‘multi’: Pass multiple values in a single INSERT clause.
  callable with signature (pd_table, conn, keys, data_iter).
```

method 파라미터에 ON CONFLICT ORM 작업을 넣으면 될 것 같아서 바로 실행에 옮겼습니다.

https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_sql.html

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

현 시점의 최신 버전인 postgres 이미지로 시작하며, 볼륨과 init-user-db.sh 파일을 지정하고 테이블 구조를 미리 작성했습니다.

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

위 init-user-db.sh 스크립트로 테이블을 미리 만듭니다.

제약조건에 충돌이 발생하면 업데이트할 수 있는지 파악하고 싶었으므로 CONSTRAINT 우선해서 작성했습니다.

```sh
chmod +x init-user-db.sh
```

스크립트로 실행 가능하도록 변경하고 컨테이너를 올립니다.

## 준비

이번 구현하는 함수는 아래와 같이 SQLTable, Engine, keys, data 정도로 받고 있습니다.

```python
def psql_insert(table, conn, keys, data_iter):
  pass
```

https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#insertion-method

해당 공식 문서를 찾아보니 테이블은 대략 table.schema, table.name 처럼 정보를 불러올 수 있고, 커넥션은 conn.connection 으로 받을 수 있었습니다.

## 패키지 준비

```
pandas==1.3.5
SQLAlchemy==1.4.29
psycopg2-binary==2.9.3
```

pandas는 기본적으로 준비하고, PostgreSQL 데이터베이스를 조작할 예정이므로 psycopg2-binary 패키지도 준비합니다.

## 코딩

데이터베이스와 의존 패키지까지 준비를 끝냈다면, 샘플 데이터도 준비합니다.

```python
from dataclasses import make_dataclass
import pandas as pd

content = make_dataclass("Content", [("id", int), ("description", str)])
dataset = pd.DataFrame([content(0, "test 1"), content(1, "test 2"), content(2, "test 3")])
```

데이터베이스에 Upsert 작업을 수행하기 위해서 테스트 데이터셋을 만듭니다.

간단하게 데이터베이스 컬럼과 동일한 구조로 데이터클레스 3 개의 행을 준비합니다.

```python
from sqlalchemy import create_engine

engine = create_engine(DB_URL)
```

DB_URL 변수에 'postgresql://~' 로 시작하는 DB 연결 정보를 넣고, 엔진을 준비합니다.

이번 케이스에서는 미리 준비해둔 PostgreSQL 도커 컨테이너의 접속 정보로 DB_URL에 입력하게 됩니다.

```python
def psql_upsert(table, conn, keys, data_iter):
```

to_sql 메소드에서 요구하는 파라미터를 그대로 작성합니다.

```python
for row in data_iter:
    data = dict(zip(keys, row))
    insert_st = insert(table.table).values(**data)
    upsert_st = insert_st.on_conflict_do_update(constraint="id_pk", set_=data)
    conn.execute(upsert_st)
```

지정된 테이블에 데이터 넣으면서 constraint 충돌날 때 원하는 값을 갱신하도록 합니다.

```python
def psql_upsert(table, conn, keys, data_iter):
    for row in data_iter:
        data = dict(zip(keys, row))
        insert_st = insert(table.table).values(**data)
        upsert_st = insert_st.on_conflict_do_update(constraint="id_pk", set_=data)
        conn.execute(upsert_st)

dataset.to_sql("article", con=engine, if_exists='append', index=False, method=psql_upsert)
```

to_sql 메소드에 엔진과 구현한 함수를 넘겨주면 구현한 로직으로 동작합니다.

---

긴 시간을 잡아서 리서치하고 구현한 내용은 아니므로 좋은 구현 방식이 아닐 수 있습니다.

만약 더 좋은 방식을 찾거나 공식적으로 Upsert를 지원하게 된다면, 해당 아티클을 업데이트하도록 하겠습니다. 

감사합니다.
