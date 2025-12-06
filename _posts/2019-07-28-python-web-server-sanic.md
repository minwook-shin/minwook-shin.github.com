---
layout: post
title: Python 웹 서버 만들어주는 Sanic 라이브러리 알아보기
---

오늘은 Python에서 웹 서버를 만드는 웹 프레임워크인 Sanic 라이브러리를 알아보려 합니다.

## Sanic 설치

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
pip3 install sanic
```

pip로 Sanic을 설치합니다.

## hello world

```python
from sanic import Sanic
from sanic.response import json
```

Sanic을 가져옵니다.

```python
app = Sanic()
```

Sanic 객체를 만듭니다.

```python
@app.route('/')
async def test(request):
    return json({'hello': 'world'})
```

json 형식의 {'hello': 'world'}를 async 반환하는 함수를 만듭니다.

데코레이터로 라우터를 지정합니다.

```python
if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8000)
```

호스트와 포트를 지정하고 서버를 구동합니다.

## redirect

```python
@app.route('/')
def handle_request(request):
    return response.redirect('/redirect')
```

라우터를 생성할 때에 redirect를 수행하려면 위와 같이 작성합니다.

위 함수가 수행되면 /redirect로 넘어가집니다.

## await

```python
@app.route("/await")
async def test_await(request):
    await asyncio.sleep(5)
    return response.text("async 5 second")
```

라우터를 생성할 때에 asyncio를 사용할 수 있습니다.

## exception

```python
@app.route("/exception")
def exception(request):
    raise ServerError("exception!")


@app.exception(ServerError)
async def test(request, exception):
    return response.json({"exception": "{}".format(exception)}, status=exception.status_code)
```

exception을 일으킬 수 있습니다.

exception 데코레이터로 예외의 형식을 json으로 지정할 수도 있습니다.

## listener

```python
@app.listener('before_server_start')
def before_start(app, loop):
    log.info("before_server_start")


@app.listener('after_server_start')
def after_start(app, loop):
    log.info("after_server_start")


@app.listener('before_server_stop')
def before_stop(app, loop):
    log.info("before_server_stop")


@app.listener('after_server_stop')
def after_stop(app, loop):
    log.info("after_server_stop")
```

listener 데코레이터로 before_server_start, after_server_start, before_server_stop, after_server_stop 단계에 각자 수행할 역할을 지정할 수 있습니다.

순서대로 before_server_start가 실행되면서 서버가 구동되고, after_server_start가 실행됩니다.

그리고 서버가 꺼지기 전에 before_server_stop가 실행되면서 서버가 종료하면, after_server_stop이 실행됩니다.
