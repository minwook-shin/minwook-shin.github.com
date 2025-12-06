---
layout: post
title: Python ini와 .env 파싱하는 python-decouple 알아보기
---

오늘은 Python으로 ini 파일이나 env 설정 파일을 파싱할 수 있는 python-decouple 패키지에 대하여 알아보려 합니다.

## 개요

앱을 재배포하지 않고, 설정을 변경할 수 있는 패키지로서 ini 확장자 파일 이거나 .env 파일인 설정 파일로 값을 불러들일 수 있습니다.

## python-decouple 설치

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
pip install python-decouple
```

pip로 python-decouple을 설치합니다.

## 설정 파일 준비

```ini
[settings]
DEBUG=True
TEMPLATE_DEBUG=True
SECRET_KEY=REALSECRETKEY
DATABASE_URL=mysql://user:password@localhost/name
```

settings.ini 파일로 저장하거나,

```env
DEBUG=True
TEMPLATE_DEBUG=True
SECRET_KEY=REALSECRETKEY
DATABASE_URL=mysql://user:password@localhost/name
```

.env 파일로 저장하면 설정 파일을 준비하는 과정은 끝났습니다.

해당 값들은 앱이 실행되고 있을 때에 변경해도 반영됩니다.

```python
from decouple import config
from unipath import Path


BASE_DIR = Path(__file__).parent

DEBUG = config('DEBUG', default=False, cast=bool)
TEMPLATE_DEBUG = DEBUG
```

.env 파일을 가지고 위와 같이 만들면 Django 설정에서도 사용할 수 있습니다.

## 값 불러오기

```python
from decouple import config
```

decouple에서 config를 가져옵니다.

```python
SECRET_KEY = config('SECRET_KEY')
print(SECRET_KEY)
```

SECRET_KEY의 값을 가져올 수 있습니다.

```python
DEBUG = config('DEBUG', default=False, cast=bool)
print(DEBUG)
```

DEBUG의 값을 가져오지만, 만약 값이 없을 경우에는 False로 설정됩니다.

이 때 cast에 의하여 bool 타입으로 값을 가져오게 됩니다.

```python
F_HOST = config('FAKE_HOST', default='localhost', cast=str)
print(F_HOST)
```

FAKE_HOST의 값을 가져오지만, 해당 값은 없으므로 str 타입으로 기본 값인 localhost가 설정됩니다.

```python
F_PORT = config('FAKE_PORT', default=8080, cast=int)
print(F_PORT)
```

FAKE_PORT의 값을 가져오지만, 해당 값은 없으므로 int 타입으로 기본 값인 8080이 설정됩니다.

## 캐스트

```python
from decouple import config
```

decouple에서 config를 가져옵니다.

```python
debug = config('DEBUG', cast=bool)
print(debug)
print(type(debug))
```

DEBUG 값을 bool 타입으로 가져옵니다.

cast에 의하여 'bool' class로 인식하게 됩니다.

```python
db_url = config('DATABASE_URL', cast=str)
print(db_url)
print(type(db_url))
```

DATABASE_URL 값을 문자열인 str 타입으로 가져옵니다.

cast에 의하여 'str' class로 인식하게 됩니다.

```python
host = config('HOST', cast=str, default='localhost')
print(host)
print(type(host))
```

HOST 값을 문자열인 str 타입으로 가져옵니다.

cast에 의하여 'str' class로 인식하게 됩니다.

```python
port = config('PORT', cast=int, default = "8080")
print(port)
print(type(port))
```

PORT 값을 정수형인 int 타입으로 가져옵니다.

cast에 의하여 'int' class로 인식하게 됩니다.
