---
layout: post
title: Python 이벤트 기반 네트워크 엔진 Twisted 라이브러리 알아보기
---

오늘은 Python으로 웹 서버, 에크 서버등을 만들 수 있는 이벤트 기반 네트워킹 엔진인 Twisted를 알아보려 합니다.

## Twisted 설치

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
pip install twisted
```

pip로 Twisted를 설치합니다.

## echo 서버

twisted로 echo 서버를 만들 수 있습니다.

```python
from twisted.internet import protocol, reactor, endpoints
```

필요한 twisted 패키지를 가져옵니다.

```python
class Echo(protocol.Protocol):
    def dataReceived(self, data):
        self.transport.write(data + "!!!".encode())
```

echo 클라이언트로부터 바이트를 받는 함수를 만듭니다.

```python
class EchoFactory(protocol.Factory):
    def buildProtocol(self, addr):
        return Echo()
```

위 Echo 함수를 반환하는 프로토콜 팩토리 함수를 만듭니다.

```python
endpoints.serverFromString(reactor, "tcp:1234").listen(EchoFactory())
reactor.run()
```

run()으로 수행하면 127.0.0.1:1234 tcp 서버를 구동할 수 있으며, 해당 주소로 echo 클라이언트에서 바이트를 보낼 수 있습니다.

```python
import socket
```

이제 echo 클라이언트를 만들어보겠습니다.

```python
with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
    s.connect(('127.0.0.1', 1234))
    s.sendall(b'Hello, world')
    data = s.recv(1024)
```

socket 패키지로 127.0.0.1:1234 에 연결해주고 바이트를 보내봅니다.

```python
print('Received', data.decode())
```

다시 반환되는 바이트를 출력합니다.

## 웹 서버

twisted로 웹 서버를 만들 수 있습니다.

```python
from twisted.web import server, resource
from twisted.internet import reactor, endpoints
```

필요한 twisted 패키지를 가져옵니다.

```python
class WebServer(resource.Resource):
    isLeaf = True

    def render_GET(self, request):
        request.setHeader(b"content-type", b"text/plain")
        content = u"this is web server."
        return content.encode("ascii")
```

Resource를 상속받은 WebServer 클래스를 만듭니다.

헤더를 설정하고 컨텐츠를 아스키코드로 인코딩해서 반환합니다.

```python
endpoints.serverFromString(reactor, "tcp:8080").listen(server.Site(WebServer()))
reactor.run()
```

8080 포트로 웹 서버를 구동할 수 있습니다.