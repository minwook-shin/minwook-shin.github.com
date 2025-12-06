---
layout: post
title: Python wsgi 웹 애플리케이션 werkzeug 라이브러리 알아보기
---

오늘은 Python에서 WSGI 웹 애플리케이션인 werkzeug 라이브러리를 알아보려 합니다.

플라스크는 Werkzeug를 랩핑하여 WSGI를 사용합니다.

## werkzeug 설치

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
pip install -U Werkzeug
```

pip로 werkzeug를 설치합니다.

## hello world 서버

```python
from werkzeug.wrappers import Request, Response
from werkzeug.serving import run_simple
```

werkzeug를 가져옵니다.

```python
@Request.application
def application(request):
    return Response('Hello, World!')
```

application 함수를 구현합니다.

이로 인하여 해당 서버에서 반환은 'Hello, World!'가 됩니다.

```python
if __name__ == '__main__':
    run_simple('localhost', 8000, application)
```

localhost의 8000번 포트를 열어서 application을 구동합니다.

## 서버 테스트

```python
from werkzeug.test import Client
from werkzeug.wrappers import BaseResponse

import hello_world
```

werkzeug와 방금 만든 hello_world 애플리케이션을 가져옵니다.

```python
c = Client(hello_world.application, BaseResponse)
resp = c.get('/')
status = resp.status_code
print(status)
```

hello_world 애플리케이션의 클라이언트 객체를 생성하고, 반환되는 status code를 확인할 수 있습니다.

200으로 반환됩니다.

```python
header = resp.headers
print(header)
```

헤더를 확인할 수 있습니다.

Content-Type: text/plain; charset=utf-8
Content-Length: 13

위와 같이 출력됩니다.

```python
response_data = resp.data
print(response_data)
```

서버에서 반환받은 값이 출력됩니다.

b'Hello, World!'로 출력됩니다.

## 서버 디버깅

```python
from werkzeug.debug import DebuggedApplication
import hello_world
```

werkzeug와 방금 만든 hello_world 애플리케이션을 가져옵니다.

```python
app = DebuggedApplication(hello_world.application, evalex=True)
```

디버깅 지원을 활성화할 수 있습니다.