---
layout: post
title: Python coroutine 기반 networking 구현을 위한 gevent 패키지 알아보기
---

오늘은 Python으로 coroutine 기반의 networking을 구현할 수 있는 gevent 패키지에 대하여 알아보려 합니다.

## 개요

이벤트 루프에 동기 API를 제공하는 coroutine 기반 Python 네트워킹 라이브러리입니다.

몽키 패치에 의해 기존의 블로킹 코드에 맞춰서 개발해도 non-blocking I/O로 확장할 수 있습니다.

## gevent 설치

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
pip install gevent
```

pip로 gevent를 설치합니다.

## 소켓

```python
import gevent
from gevent import socket
```

gevent와 socket을 가져옵니다.

```python
urls = ['www.google.com', 'www.python.org']
```

url들을 저장합니다.

```python
jobs = [gevent.spawn(socket.gethostbyname, url) for url in urls]
```

spawn으로 Greenlet 객체를 만들고 함수를 수행합니다.

```python
gevent.joinall(jobs, timeout=2)
```

greenlets이 끝날 때까지 기다립니다.

```python
content = [job.value for job in jobs]
print(content)
```

얻은 값을 출력합니다.

## 몽키 패치

```python
import gevent
from gevent import monkey
```

gevent와 몽키 패치를 위해 monkey를 가져옵니다.

```python
monkey.patch_all()
```

몽키 패치는 실행중인 프로그램의 메모리 소스를 바꾸는 것으로서, non-blocking으로 바꾸어 주는 역할을 합니다.

```python
import requests
```

requests 라이브러리를 가져옵니다.

pip install requests 를 수행해야 합니다.

```python
urls = [
    'https://www.google.com/',
    'https://www.python.org/'
]
```

```python
def print_header(url):
    data = requests.get(url).text
    print('%s:  %r' % (url, data))

jobs = [gevent.spawn(print_header, _url) for _url in urls]
gevent.wait(jobs)
```

## 쓰레드

```python
import time
import gevent
from gevent.threadpool import ThreadPool
```

gevent와 미리 쓰레드를 할당할 수 있는 ThreadPool을 가져옵니다.

```python
pool = ThreadPool(3)
```

3개의 ThreadPool을 할당합니다.

```python
for _ in range(4):
    pool.spawn(time.sleep, 1)
```

그리고 time.sleep 함수를 할당합니다.

```python
gevent.wait()
```

이벤트 루프가 끝날 때까지 기다립니다.

## 프로세스

```python
import gevent
from gevent import subprocess
```

gevent와 새로운 프로세스를 만들고 파이프로 입출력할 수 있는 subprocess를 가져옵니다.

```python
p1 = subprocess.Popen(['uname'], stdout=subprocess.PIPE)
p2 = subprocess.Popen(['ls'], stdout=subprocess.PIPE)

gevent.wait([p1, p2], timeout=2)
```

기본적인 프로세스 생성을 Popen에서 담당하고, wait로 이벤트 루프가 끝날 때까지 기다립니다.

```python
if p1.poll() is not None:
    print('%r' % p1.stdout.read())
if p2.poll() is not None:
    print('%r' % p2.stdout.read())
```

poll로 프로세스가 종료되었는지 확인하면서 None이 아니라면 값을 출력합니다.

```python
p1.stdout.close()
p2.stdout.close()
```

stdout를 닫습니다.
