---
layout: post
title: Python concurrent networking 구현을 위한 eventlet 패키지 알아보기
---

오늘은 Python으로 concurrent networking을 구현할 수 있는 eventlet 패키지에 대하여 알아보려 합니다.

## 개요

기존의 블로킹 코드에 맞춰서 개발해도 non-blocking I/O으로 확장할 수 있다고 합니다.

## eventlet 설치

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
pip install eventlet
```

pip로 eventlet을 설치합니다.

## 크롤러

```python
import eventlet
from eventlet.green.urllib import request
```

eventlet에 맞춰진 urllib에서 request를 가져옵니다.

기존의 urllib.request와 사용하는 방식은 비슷합니다.

```python
urls = [
    "https://www.google.com/intl/en_ALL/images/logo.gif",
    "http://python.org/images/python-logo.gif",
]
```

가져올 사진 url들을 정의합니다.

```python
def fetch(text):
    content = request.urlopen(text).read()
    print("done : ", text)
    return text, content
```

기존의 request와 같이 urlopen를 사용하여 컨텐츠를 가져옵니다.

```python
pool = eventlet.GreenPool(200)
```

concurrency(동시성)을 제어하는 풀을 만듭니다.

```python
for url, body in pool.imap(fetch, urls):
    print("got body from", url)
```

pool.imap은 itertools.imap과 같으며, 자세한 설명은 [파이썬 공식 문서 링크](https://docs.python.org/2/library/itertools.html#itertools.imap)를 걸어둡니다.

## wgsi 서버

```python
import eventlet
from eventlet import wsgi
```

eventlet에서 wsgi를 가져옵니다.

```python
def main(_env, response):
    response('200 OK', [('Content-Type', 'text/plain')])
    return ['Hello, World!']
```

접근했을 때에 Hello, World!를 출력하는 WSGI 어플리케이션 함수를 구현합니다.

```python
wsgi.server(eventlet.listen(('127.0.0.1', 8080)), main)
```

서버 소켓에서 요청을 처리하는 WSGI 서버를 시작할 수 있습니다.

## echo 서버

```python
import eventlet
```

eventlet을 가져옵니다.

```python
def main(fd):
    while True:
        x = fd.readline()
        if not x:
            break
        fd.write(x)
        fd.flush()
        print(x, end=' ')
```

클라이언트와 연결되어 while문에 진입하면 readline으로 텍스트를 읽고, write와 flush로 버퍼에 저장되있는 값을 저장해서 클라이언트로 보내줍니다.

```python
server = eventlet.listen(('127.0.0.1', 5000))
pool = eventlet.GreenPool()
```

로컬 서버와 concurrency(동시성)을 제어하는 풀을 만듭니다.

```python
while True:
    new_sock, address = server.accept()
    pool.spawn_n(main, new_sock.makefile('rw'))
```

클라이언트의 요청을 수락하고, 소켓과 주소를 받아옵니다.

그리고 green thread에서 함수를 수행합니다.
