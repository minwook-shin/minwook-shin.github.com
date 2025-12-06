---
layout: post
title: Python Django 프로젝트에 부트스트랩4 추가하여 사용하기
---

오늘은 Python django 프로젝트에 부트스트랩4를 추가하여 스타일을 꾸밀 수 있는 django-bootstrap4를 적용해보려 합니다.

## 깃허브

해당 django-bootstrap4의 깃허브 주소는 아래와 같습니다.

https://github.com/zostera/django-bootstrap4

문서들은 아래 주소에 있습니다.

https://django-bootstrap4.readthedocs.io/

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
pip install django-bootstrap4
```

django-bootstrap4를 설치합니다.

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

## django-bootstrap4 적용

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'bootstrap4'
]
```

프로젝트의 settings.py 파일에서 INSTALLED_APPS를 위와 같이 고쳐줍니다.

INSTALLED_APPS는 'bootstrap4'를 추가합니다.

```python
# -*- coding: utf-8 -*-
from django.shortcuts import render


def home(request):
    return render(request, "app/home.html")
```

views.py 파일에서 render 함수로 html 파일을 사용합니다.

html 파일은 [해당 링크](https://github.com/zostera/django-bootstrap4/blob/master/demo/templates/app/home.html)처럼 사용할 수 있습니다.

```python
from django.contrib import admin
from django.urls import path

from .views import home

urlpatterns = [
    path('admin/', admin.site.urls),
    path('home/', home, name='home'),
]
```

urls.py 파일에서 views 파일에서 가져온 함수를 특정 url과 연결합니다.

## 서버 구동

```
python3 manage.py runserver
```

지금까지 프로젝트에 django-bootstrap4를 적용했으므로 manage.py 파일으로 서버를 다시 실행합니다.
