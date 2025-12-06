---
layout: post
title: Python Django 프로젝트에 template 생성하기
---

오늘은 Python django 프로젝트에서 template을 만들어서 html으로 hello world를 출력하는 실습을 해보려 합니다.

## 이전 포스트

관리자 페이지 접근까지 실습을 진행한 블로그 글은 [이 곳](https://minwook-shin.github.io/python-django-getting-started/)에 있습니다.

이 포스트에서는 이미 django 프로젝트를 생성하고 앱과 연결을 마무리지었다는 것을 가정하고 시작합니다.

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

## url 설정

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.List, name='List'),
]
```

앱의 urls.py에서 먼저 views 파일을 연결해줍니다.

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('test.urls')),
]
```

프로젝트의 urls.py에서는 방금 만들어둔 앱 urls.py를 django.urls의 include로 넣어줍니다.

이 때 코드에서는 도메인의 최상단으로 지정했습니다.

## 동적 데이터

이전에 준비해둔 데이터베이스의 내용을 웹페이지로 꺼내야 합니다.

```python
from django.shortcuts import render
from .models import Test


# Create your views here.
def List(request):
    testText = Test.objects.all()
    return render(request, 'list.html', {'testText': testText})
```

앱의 views.py에서 (이전 url에서 지정한대로) views.postList이라는 함수를 구현해줍니다.

list.html 파일은 아래에서 구현할 것이며, 해당 html 파일에는 데이터베이스의 모든 내용을 가져오는 쿼리셋으로 저장한 변수를 보내줍니다.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <title>Page Title</title>
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <link rel="stylesheet" type="text/css" media="screen" href="main.css" />
    <script src="main.js"></script>
  </head>

  <body>
    hello, world!
    <br />
    {% for t in testText %}
    <p>{{ t.author }}</p>
    <p>{{ t.text }}</p>
    {% endfor %}
  </body>
</html>
```

앱에 templates이라는 폴더를 만들고 list라는 html 파일도 생성해줍니다.

템플릿 문법에 의해 for문을 사용할 수 있으며, 각 모델의 속성을 따로 출력할 수 있습니다.

## 서버 확인

```
python3 manage.py runserver
```

서버를 다시 구동해줍니다.

127.0.0.1:8000 으로 접속하면 hello, world와 미리 저장해둔 데이터베이스의 내용이 포함된 페이지가 출력됨을 알 수 있습니다.
