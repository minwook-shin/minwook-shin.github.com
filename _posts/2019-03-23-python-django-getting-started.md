---
layout: post
title: Python Django 프로젝트를 생성하고, 관리자 페이지에 접속하기
---

오늘은 Python django 프로젝트를 생성해보고 관리자 페이지에 접속하는 간단한 실습을 해보려 합니다.

## virtualenv

우선 새로운 django 프로젝트 폴더에 virtualenv로 파이썬 환경을 분리해줍니다.

프로젝트마다 다른 Python 의존성 패키지들을 각각의 독립적인 프로젝트 폴더에 따로 제공해줄 수 있습니다.

```
pip3 install virtualenv
```

pip로 설치할 수 있습니다.

## 가상환경 설정

```
virtualenv -mvenv env
```

```
source env/bin/activate
```

프로젝트를 만들 폴더에 가상환경을 만들고 활성화합니다.

## 가상환경에 django 패키지 설치하기

```
pip3 install --upgrade pip
```

```
pip3 install django
```

pip를 업그레이드하고, django를 설치해줍니다.

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

```
python3 manage.py runserver
```

지금까지 프로젝트를 만들었으므로 manage.py 파일으로 서버를 실행합니다.

## setting.py 설정

```python
LANGUAGE_CODE = 'ko-kr'

TIME_ZONE = 'Asia/Seoul'

USE_I18N = True

USE_L10N = True

USE_TZ = True
```

setting.py 파일에서 Internationalization 부분을 고쳐줍니다.

LANGUAGE_CODE에 의해 django 프로젝트 홈페이지의 언어가 바뀌며, TIME_ZONE에 의해 시간대가 바뀝니다.

```python
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
```

홈페이지에 사용될 static 파일들의 경로를 추가해줍니다.

## 앱 생성 및 연결

```
python3 manage.py startapp test
```

프로젝트에 추가할 앱을 생성해줍니다.

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'test',
]
```

setting.py 파일의 INSTALLED_APPS에 앱의 이름을 추가해주어야 프로젝트와 앱이 연결됩니다.

## 모델 생성 및 연결

```python
from django.db import models


# Create your models here.
class Test(models.Model):
    author = models.ForeignKey('auth.User', on_delete=models.CASCADE)
    text = models.TextField()

    def publish(self):
        self.save()

    def __str__(self):
        return self.title
```

모델을 생성해주기 위해 앱의 models.py 파일에서 Model 클래스로 구성해줍니다.

속성과 메소드를 구성하고, publish 메소드와 \_\_str\_\_ 메소드도 만들어줍니다.

ForeignKey는 다른 모델을 링크하므로 User에 대한 정보를 가져옴을 알 수 있습니다.

```
python3 manage.py makemigrations test
```

```
python3 manage.py migrate test
```

앱의 데이터베이스에 대한 마이그레이션 파일을 준비해주고, 곧바로 프로젝트에 앱의 데이터베이스를 마이그레이션해줍니다.

```python
from django.contrib import admin

# Register your models here.
from .models import Test

admin.site.register(Test)
```

앱의 admin.py 파일에서 모델을 등록해줍니다.

models 파일에서 Test를 가져와서 register하게 됩니다.

이로서 Test라는 모델이 djago 프로젝트에 반영됬습니다.

## 관리자 계정 생성

```
python manage.py createsuperuser
```

createsuperuser 명령으로 관리자의 이름, 주소, 비밀번호를 구성할 수 있습니다.

## 관리자 페이지 확인

```
python3 manage.py runserver
```

서버를 다시 구동해줍니다.

127.0.0.1:8000/admin 으로 접속하고 위에서 설정한 관리자 이름과 비밀번호를 입력하면 정상적으로 관리자 페이지가 출력됨을 알 수 있습니다.
