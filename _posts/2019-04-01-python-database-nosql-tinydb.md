---
layout: post
title: Python TinyDB로 데이터베이스 사용하기
---

오늘은 Python에서 사용할 수 있는 TinyDB를 이용하여 데이터 보관해보려 합니다.

## 개요

TinyDB는 NoSQL로서 dict 형태의 문서로 저장할 수 있는 데이터베이스입니다.

## 설치

우선 virtualenv로 파이썬 환경을 분리해줍니다.

```
pip3 install virtualenv
```

```
virtualenv -mvenv env
```

env라는 이름의 가상 환경을 생성합니다.

```
source env/bin/activate
```

프로젝트를 만들 폴더에서 활성화합니다.

```
pip install tinydb
```

가상환경을 구성한 뒤에 pip로 tinydb를 설치합니다.

## 실습

```python
from tinydb import TinyDB, Query, where
```

tinydb 패키지에서 객체를 생성할 TinyDB와 쿼리문을 적용할 Query, 고전적인 방식으로 데이터를 찾을 where을 가져옵니다.

```python
from tinydb.storages import MemoryStorage
db = TinyDB(storage=MemoryStorage)
```

만약 메모리에 저장되는 DB로 진행하려면 storage를 메모리로 저장하게 합니다.

```python
db = TinyDB('db.json')
```

db.json라는 파일로 데이터베이스를 만듭니다.

```python
table = db.table('table_name')
```

테이블을 만들어서 데이터베이스를 여러개로 나눌 수 있습니다.

```python
db.purge()
```

데이터베이스를 지웁니다.

```python
db.insert({'name': 'Kim', 'score': 90})
db.insert({'name': 'Shin', 'score': 80})
db.insert({'wrong-value': True})
```

dict 형태로 저장하게 됩니다.

```python
db.insert_multiple([
    {'name': 'Lee', 'score': 80},
    {'name': 'Han', 'score': 50}])
```

여러 dict의 묶음을 한 번에 저장하게 됩니다.

```python
db.upsert({'name': 'Han', 'score': 100}, Query().name == 'Han')
```

만약 쿼리문과 일치하는 데이터가 있다면 해당 문서가 업데이트되고, 없다면 새로 쓰여지게 됩니다.

```python
print(db.all())
```

지금까지 작업한 뒤에 데이터베이스의 모든 내용을 출력합니다.

```python
for i in db:
    print(i)
```

for문에 의해 순서대로 출력합니다.

```python
print("name", db.search(Query().name == 'Shin'))
print("score", db.search(Query().score >= 90))
```

Query() 객체에서 속성이 일치하는 문서를 찾아 반환합니다.

```python
print("array indexing", db.search(Query()["name"] == 'Shin'))
```

인덱스로 찾는 방식도 됩니다.

```python
print("traditional", db.search(where('name') == 'Shin'))
```

where을 패키지에서 가져와서 사용할 수도 있습니다.

```python
db.remove(~(Query().name.exists()))
print(db.all())
```

remove를 사용하면 특정 조건에 부합하는 문서를 삭제할 수 있습니다.

위 코드는 물결(~)에 의해서 name이라는 속성이 없는 문서만 지우게 됩니다.

```python
db.update({'score': 0}, Query().name == 'Han')
print(db.all())
```

조건에 맞는 문서를 업데이트할 수 있습니다.

위 코드는 name이 Han인 문서를 찾아 score라는 속성의 값을 바꿉니다.

```python
shin_account = db.get(Query().name == 'Shin')
print(shin_account)
```

특정 조건에 맞는 문서를 가져와서 출력할 수 있습니다.

```python
docs = db.search(Query().name == 'Shin')
for doc in docs:
    doc['score'] = 100
db.write_back(docs)
docs = db.search(Query().name == 'Shin')
print(docs)
```

특정 조건에 맞는 문서를 가져와서 변경한 뒤에 write_back으로 다시 쓸 수 있습니다.
