---
layout: post
title: Python 웹 프레임워크 Pyramid 라이브러리 알아보기
---

오늘은 Python에서 웹 서비스를 제작할 때에 사용할 수 있는 웹 프레임워크인 Pyramid 라이브러리를 알아보려 합니다.

## Pyramid 설치

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
pip install pyramid
```

pip로 pyramid를 설치합니다.

## hello world

```python
from wsgiref.simple_server import make_server
from pyramid.config import Configurator
from pyramid.response import Response
```

pyramid와 wsgiref를 가져옵니다.

```python
def hello_world(request):
    return Response('Hello World!')
```

hello_world라는 함수로 'Hello World!' Response를 반환하게 합니다.

```python
if __name__ == '__main__':
    with Configurator() as config:
        config.add_route('hello', '/')
        config.add_view(hello_world, route_name='hello')
        app = config.make_wsgi_app()
    server = make_server('0.0.0.0', 8000, app)
    server.serve_forever()
```

hello 라우터를 추가하고, 서버에 접근하면 hello_world 함수가 수행되게 합니다.

## get

```python
from wsgiref.simple_server import make_server
from pyramid.config import Configurator
from pyramid.response import Response
```

pyramid와 wsgiref를 가져옵니다.

```python
def hello_world(request):
    name = request.params.get('id', 'No id')
    return Response(
        content_type="text/plain",
        body="{} {}".format(request.url, str(name))
    )
```

hello_world 함수로 id라는 파라미터를 받아 Response를 반환하게 할 수 있습니다.

```python
if __name__ == '__main__':
    with Configurator() as config:
        config.add_route('hello', '/')
        config.add_view(hello_world, route_name='hello')
        app = config.make_wsgi_app()
    server = make_server('0.0.0.0', 8000, app)
    server.serve_forever()
```

hello 라우터를 추가하고, 서버에 접근하면 hello_world 함수가 수행되게 합니다.

이 때에 http://127.0.0.1:8000/?id=test_id 에 접근하면 주소와 쿼리 값이 출력됩니다.

## views

```python
from wsgiref.simple_server import make_server
from pyramid.config import Configurator
from pyramid.httpexceptions import HTTPFound
from pyramid.response import Response
from pyramid.view import view_config
```

pyramid와 wsgiref를 가져옵니다.

```python
@view_config(route_name='home')
def home_view(request):
    return Response('<p>Visit <a href="/login">login page</a></p>')
```

데코레이터로 라우터 이름을 정하고, view 함수를 만듭니다.

해당 함수는 Response를 반환합니다.

```python
@view_config(route_name='login')
def login_view(request):
    return HTTPFound(location="/")
```

데코레이터로 라우터 이름을 정하고, view 함수를 만듭니다.

해당 함수는 HTTPFound를 반환하므로 다시 home으로 돌아가게 됩니다.

```python
if __name__ == '__main__':
    with Configurator() as config:
        config.add_route('home', '/')
        config.add_route('login', '/login')
        config.scan('view')
        app = config.make_wsgi_app()
    server = make_server('0.0.0.0', 8000, app)
    server.serve_forever()
```

home, login 라우터를 추가하고, 위 뷰 함수들이 들어있는 파일의 이름을 스캔하도록 합니다.

위 코드를 예시로 들면 view.py에 뷰 함수들이 존재해야 합니다.

그리고 서버를 구동합니다.