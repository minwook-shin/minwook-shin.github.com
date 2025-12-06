---
layout: post
title: Python json 기반의 데이터베이스 pickleDB 라이브러리 알아보기
---

오늘은 Python을 가지고 json 형식 기반의 데이터베이스 pickleDB 패키지에 대하여 알아보려 합니다.

## pickleDB 설치

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
pip3 install --upgrade pip
```

pip의 업그레이드가 존재하는지 확인하고 진행합니다.

```
pip install pickledb
```

pip로 pickleDB를 설치합니다.

## pickle db 기본 사용법

```python
import pickledb
```

pickledb를 가져옵니다.

```python
db = pickledb.load('test.db', False)
```

load로 데이터베이스를 제어할 pickledb 객체를 반환합니다.

```python
db.set('key', 'value')
```

키의 문자열 값을 추가합니다.

```python
is_key = db.exists('key')
print(is_key)
```

key라는 키가 존재하면 bool 타입으로 반환합니다.

```python
value = db.get('key')
print(value)
```

키의 값을 가져옵니다.

```python
value_all = db.getall()
print(value_all)
```

데이터베이스의 안에 있는 모든 키를 가져옵니다.

```python
db.set('key1', 'temp_value')

value_all = db.getall()
print(value_all)

db.rem('key1')

value_all = db.getall()
print(value_all)
```

rem으로 특정 키를 지울 수 있습니다.

```python
total = db.totalkeys()
print(total)
```

모든 키의 수를 카운트합니다.

```python
db.append('key','_test')
value_all = db.get('key')
print(value_all)
```

기존 키의 값에 값을 더 추가합니다.

```python
db.lcreate("test_list")
db.ladd('test_list','val')
print(db.lget('test_list',0))
```

리스트가 값인 키를 추가합니다.

```python
is_dump = db.dump()
print(is_dump)
```

메모리에 존재하는 db를 파일로 저장합니다.
