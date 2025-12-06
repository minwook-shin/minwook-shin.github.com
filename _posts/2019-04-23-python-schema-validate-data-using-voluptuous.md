---
layout: post
title: Python 스키마로 데이터 유효성 검사하는 Voluptuous 라이브러리 알아보기
---

오늘은 Python으로 사용자가 정의한 스키마로 데이터의 유효성을 검증할 수 있는 Voluptuous 패키지에 대하여 알아보려 합니다.

## Voluptuous 설치

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
pip install voluptuous
```

pip로 Voluptuous를 설치합니다.

## hello world

간단한 예제입니다.

```python
from voluptuous import Schema

schema = Schema({
    'hello': str,
})


schema({'hello': 'world'})
```

hello라는 키를 가진 스키마에 따라 데이터를 검증할 수 있습니다.

## type

타입마다 다르게 스키마를 설정할 수 있습니다.

```python
from voluptuous import Schema
from voluptuous import Url
```

Schema를 가져오고, Url도 주소 형식을 정의하기 위해서 가져옵니다.

```python
schema = Schema(10)
num = schema(10)
print(num)
```

숫자 10이 아니면 올바르지 않는 값이라고 오류를 발생합니다.

```python
schema = Schema('string')
string = schema('string')
print(string)
```

string이라는 문자열이 아니면 올바르지 않는 값이라고 오류를 발생합니다.

```python
schema = Schema(int)
int_type = schema(1)
print(int_type)
```

하지만 int라고 정수형 타입만 정의하면 어느 숫자가 와도 괜찮습니다.

```python
schema = Schema(Url())
url_type = schema('http://python.org')
print(url_type)
```

url 형식도 데이터 검증을 할 수 있습니다.

```python
schema = Schema([10, 'test', 'string'])
ex1 = schema([10])
ex2 = schema([10, 'test'])
print(ex1)
print(ex2)
```

리스트에 정의되어 있는 값들이 들어있지 않으면 올바르지 않는 값이라고 오류를 발생합니다.

```python
schema = Schema({1})
set = schema({1})
print(set)
```

셋도 스키마로 정의하여 데이터를 검증할 수 있습니다.

```python
schema = Schema({'hello': 'world'})
dict_type = schema({'hello': 'world'})
print(dict_type)
```

딕셔너리 타입도 스키마로 검증할수 있습니다.

## function

```python
from datetime import datetime
from voluptuous import Schema


def check_date(fmt='%Y-%m-%d'):
    return lambda v: datetime.strptime(v, fmt)


schema = Schema(check_date())
schema('2019-04-23')
```

유효성 검사 함수로 스키마를 구성할 수 있습니다.
