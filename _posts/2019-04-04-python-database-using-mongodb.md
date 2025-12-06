---
layout: post
title: Python MongoDB 연결해서 사용해보기
---

오늘은 Python에서 MongoDB를 연결해주는 PyMongo에 대하여 알아보려 합니다.

## 개요

MongoDB는 NoSQL로서 dict 형태의 문서로 저장할 수 있는 데이터베이스이며, PyMongo는 MongoDB의 파이썬 드라이버입니다.

## MongoDB 설치

```
apt-get install mongodb-clients mongodb-server
```

우분투의 경우에는 mongodb 클라이언트와 서버를 같이 설치합니다.

```
brew install mongodb
```

맥의 경우에는 brew를 이용하여 설치합니다.

```
mongod --config /usr/local/etc/mongod.conf
```

위 명령어를 터미널에 입력하면 해당 터미널이 꺼질 때까지만 mongodb 서비스가 구동됩니다.

```
brew services start mongodb
```

위 명령어를 터미널에 입력하면 로그인할 때마다 mongodb 서비스가 구동됩니다.

```
brew services stop mongodb
```

만약 mongodb 서비스를 중지하고 싶다면 stop을 하면 됩니다.

```
vim /usr/local/etc/mongod.conf
```

데이터베이스의 저장 위치를 바꾸기 위해서 mongod.conf 파일을 고칩니다.

```json
systemLog:
  destination: file
  path: /usr/local/var/log/mongodb/mongo.log
  logAppend: true
storage:
  dbPath: /Users/(사용자 이름)/data/db
net:
  bindIp: 127.0.0.1
```

storage의 dbPath를 원하는 위치로 변경합니다.

## PyMongo 설치

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

가상환경을 폴더에서 활성화합니다.

```
python -m pip install pymongo
```

pip로 PyMongo를 설치합니다.

## 실습

```python
from pymongo import MongoClient
```

pymongo 패키지를 가져옵니다.

```python
client = MongoClient('localhost', 27017)
```

localhost의 27017포트로 mongod 인스턴스와 연결합니다.

```python
db = client.test_database
# or
db = client['test-database']
```

속성 스타일로 test_database라는 데이터베이스를 만듭니다.

속성 스타일 말고도 딕셔너리 스타일로 데이터베이스를 만들 수도 있습니다.

```python
client.drop_database('test_database')
```

만약 데이터베이스를 지우려면 drop_database를 사용하면 됩니다.

```python
user = {"name": "Kim",
        "birthday": 0000}
```

MongoDB는 JSON-스타일을 사용하므로 user라는 딕셔너리를 만듭니다.

```python
users = db.users
user_id = users.insert_one(user).inserted_id
```

users라는 collection을 만들고, user 딕셔너리를 넣습니다.

```python
print(user)
```

\_id값을 딕셔너리에 넣지 않았지만, 자동으로 추가됨을 확인할 수 있습니다.

```python
print(users.find_one())
```

find_one은 단일 문서를 반환합니다.

```python
print(users.find_one({"name": "Kim"}))
print(users.find_one({"_id": user_id}))
```

특정 엘리먼트만을 이용하여 문서를 찾을 수 있습니다.

이전에 저장해둔 user_id로도 찾을 수도 있습니다.

```python
user_many = [{"name": "Lee",
                    "birthday": 1111},
                   {"name": "Shin",
                    "birthday": 2222}]

user_ids = users.insert_many(user_many)
```

insert_many로 많은 딕셔너리 문서들을 넣습니다.

```python
for i in users.find():
    print(i)
```

users의 많은 문서를 가져오려면 find를 사용하며, Cursor 인스턴스로 인하여 반복할 수 있습니다.
