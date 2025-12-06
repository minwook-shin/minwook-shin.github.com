---
layout: post
title: Python Django 프로젝트의 관리자 페이지 인터페이스를 Grappelli로 변경하기
---

오늘은 Python django 프로젝트의 관리자 페이지의 인터페이스를 스타일링해주는 grappelli을 적용해보려 합니다.

## 깃허브

해당 grappelli의 깃허브 주소는 아래와 같습니다.

https://github.com/sehmaschine/django-grappelli

문서들은 아래 주소에 있습니다.

http://readthedocs.org/docs/django-grappelli/

## 버전

- Grappelli 2.12.3 : Django 2.1

- Grappelli 2.10.5 : Django 1.11

해당 포스트를 업로드하는 날짜(2019년 3월 29일) 기준으로 2.12.3이 최신입니다.

## virtualenv

우선 새로운 django 프로젝트 폴더에 virtualenv로 파이썬 환경을 분리해줍니다.

프로젝트마다 다른 Python 의존성 패키지들을 각각의 독립적인 프로젝트 폴더에 따로 제공해줄 수 있습니다.

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

## 가상환경에 django 패키지 설치하기

```
pip3 install --upgrade pip
```

```
pip3 install django
```

pip를 업그레이드하고, django를 설치합니다.

## 설치

```
pip install django-grappelli
```

django-grappelli을 설치합니다.

## django 프로젝트 생성

```
django-admin startproject testProject .
```

django 프로젝트를 django-admin으로 생성합니다.

위 명령어에서는 현재 폴더에 testProject라는 이름으로 생성하게 됩니다.

## 데이터베이스 생성

```
python3 manage.py migrate
```

데이터베이스를 sqlite3로 마이그레이션해줄 수 있습니다.

## 서버 최초 실행

```
python3 manage.py runserver
```

지금까지 프로젝트를 만들었으므로 manage.py 파일으로 서버를 실행합니다.

## 관리자 계정 생성

```
python manage.py createsuperuser
```

서버를 중지하고, createsuperuser 명령으로 관리자의 이름, 주소, 비밀번호를 구성합니다.

## 관리자 페이지 확인

```
python3 manage.py runserver
```

서버를 다시 구동해줍니다.

127.0.0.1:8000/admin 으로 접속하고 위에서 설정한 관리자 이름과 비밀번호를 입력하면 정상적으로 관리자 페이지가 출력됨을 알 수 있습니다.

## grappelli 적용

```python
INSTALLED_APPS = [
    'grappelli',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.request',
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

프로젝트의 settings.py 파일에서 INSTALLED_APPS, TEMPLATES를 위와 같이 고쳐줍니다.

INSTALLED_APPS는 'grappelli', TEMPLATES은 'django.template.context_processors.request'를 추가합니다.

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('grappelli/', include('grappelli.urls')),
    path('admin/', admin.site.urls),
]
```

프로젝트의 urls.py 파일에서 grappelli에 대한 url를 추가합니다.

## 미디어 파일

```python
python manage.py collectstatic
```

미디어 파일을 수집합니다.

## 서버 구동

```
python3 manage.py runserver
```

지금까지 프로젝트에 grappelli를 적용했으므로 manage.py 파일으로 서버를 다시 실행합니다.
