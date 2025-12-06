---
layout: post
title: Python Django 프로젝트의 관리자 페이지 커스터마이징하기
---

오늘은 Python django 프로젝트의 관리자 페이지를 커스터마이징을 해보려고 합니다.

## 이전 포스트

관리자 페이지 접근까지 실습을 진행한 블로그 글은 [이 곳](https://minwook-shin.github.io/python-django-getting-started/)에 있습니다.

이 포스트에서는 이미 django 프로젝트를 생성하고 앱의 데이터베이스 생성 과정을 마무리지었다는 것을 가정하고 시작합니다.

## 데이터베이스 준비

```python
from django.db import models


# Create your models here.
class DB(models.Model):
    test = models.CharField(max_length=15)
    permission = models.CharField(max_length=15,choices=(('test', "test"),('real', "real")),blank=True, null=True)

    def __str__(self):
        return self.test
```

모델을 생성해주기 위해 앱의 models.py 파일에서 Model 클래스로 구성해줍니다.

속성과 \_\_str\_\_ 메소드도 만들어줍니다.

```
python3 manage.py makemigrations [앱 이름]
```

```
python3 manage.py migrate [앱 이름]
```

앱의 데이터베이스에 대한 마이그레이션 파일을 준비해주고, 곧바로 프로젝트에 앱의 데이터베이스를 마이그레이션해줍니다.

## 관리자 url 변경하기

```python
from django.contrib import admin
from django.urls import path

urlpatterns = [
    path('custom-admin/', admin.site.urls),
]
```

기본적으로 /admin 이었던 관리자 페이지의 경로를 변경할 수 있습니다.

## 관리자 페이지 오버라이딩

[해당 글](https://docs.djangoproject.com/ko/2.1/intro/tutorial07/#customizing-your-project-s-templates)의 설명으로 코드를 대체합니다.

'admin/index.html' 파일을 extends해준다는 선언으로 새 문서 최상단에 기입한 뒤에 위 내용을 넣어주면 기존의 admin/index.html 파일을 확장해서 원하는 html 태그를 추가하거나 바꿀 수 있습니다.

django 프로젝트의 templates 폴더에서 admin를 만들어서 넣으면 됩니다.

## 관리자 페이지 문자열 변경

```python
from django.contrib import admin
from .models import DB

# Register your models here.

admin.site.site_header = "Custom Admin"
admin.site.site_title = "Custom Admin Portal"
admin.site.index_title = "Welcome to Custom Admin Portal"
```

헤더나, 제목과 같은 문자열은 위처럼 직접 html 파일을 작성하는 것보다 admin.py 파일에서 고치는 것이 더 편합니다.

site_header, site_title, index_title을 적용할 수 있습니다.

## 기본 그룹 등록 해제

```python
from django.contrib import admin
from .models import DB
from django.contrib.auth.models import Group

# Register your models here.

admin.site.unregister(Group)

admin.site.register(DB)
```

기본적으로 등록되있는 Group을 unregister해줄 수 있습니다.

## 모델 리스트 커스터마이징

```python
from django.contrib import admin
from .models import DB
from django.utils.safestring import mark_safe


# Register your models here.
class MemberAdmin(admin.ModelAdmin):
    list_per_page = 5
    list_display = ('id', 'test', 'permission')
    list_editable = ('permission',)
    list_filter = ('permission',)

admin.site.register(DB, MemberAdmin)
```

페이지당 갯수, 필드 정의, 데이터 필터 등을 admin.py 파일에서 고칠 수 있습니다.

## 검색창 추가

```python
from django.contrib import admin
from .models import DB


# Register your models here.
class MemberAdmin(admin.ModelAdmin):
    search_fields = ('test',)

admin.site.register(DB, MemberAdmin)
```

어떤 데이터를 검색할 지에 대하여 지정할 수 있습니다.

## 데이터 순서

```python
from django.contrib import admin
from .models import DB


# Register your models here.
class MemberAdmin(admin.ModelAdmin):
    ordering = ('id', 'test', 'permission',)

admin.site.register(DB, MemberAdmin)
```

각 데이터 필드들 중에서 정렬할 수 있는 필드를 지정할 수 있습니다.

## 필드셋

```python
from django.contrib import admin
from .models import DB


# Register your models here.
class MemberAdmin(admin.ModelAdmin):
    fieldsets = (('기본', {'fields': (('test',),), },), ('범위', {'fields': (('permission',),), },))

admin.site.register(DB, MemberAdmin)
```

데이터를 추가할 때에 필요한 폼들이 많을 때는 필드셋을 설정하여 분할해줍니다.

## 액션 추가

```python
from django.contrib import admin
from .models import DB
from django.contrib.auth.models import Group
from django.utils.safestring import mark_safe


# Register your models here.
class MemberAdmin(admin.ModelAdmin):
    def mark_real(self, request, queryset):
        queryset.update(permission='real')

    mark_real.short_description = "test/real 범위 변경"
    actions = ["mark_real"]

admin.site.register(DB, MemberAdmin)
```

각자의 데이터들을 선택하고 수행할 액션을 정의할 수 있습니다.

short_description으로 관리자에 직접 보일 명칭을 작성하고, actions으로 등록하면 끝납니다.

위 코드는 선택된 데이터에서 permission의 데이터를 변경해주는 액션을 해주게 됩니다.

## 서버 확인

```
python3 manage.py runserver
```

서버를 구동해줍니다.

127.0.0.1:8000 으로 접속하면 hello, world와 미리 저장해둔 데이터베이스의 내용이 포함된 페이지가 출력됨을 알 수 있습니다.
