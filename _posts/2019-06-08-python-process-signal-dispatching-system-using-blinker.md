---
layout: post
title: Python 이벤트 브로트캐스팅하는 Blinker 라이브러리 알아보기
---

오늘은 Python으로 이벤트(신호)를 브로드캐스팅할 수 있는 Blinker를 알아보려 합니다.

## Blinker 설치

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
pip install blinker
```

pip로 Blinker를 설치합니다.

## hello world

```python
from blinker import signal
```

signal을 가져옵니다.

```python
started = signal('started')
```

이름있는 신호 객체를 만듭니다.

```python
def round_function(num):
    print("Round : %s" % num)


def round_two(num):
    print(num)
```

보낸 신호를 수신할 때에 실행할 두 함수를 만듭니다.

```python
started.connect(round_function)
started.connect(round_two, sender=2)
```

send 함수로 수신할 수 있는 receiver 함수를 연결합니다.

```python
for i in range(1, 4):
    started.send(i)
```

1부터 4까지 송신합니다.

결과적으로 2가 송수신될 때는 round_two 함수도 수행됩니다.

## signal

```python
from blinker import signal
```

signal을 가져옵니다.

```python
def subscribe_function(sender):
    print(sender)

sig = signal('ready')
sig.connect(subscribe_function)
```

신호를 수신할 때 수행할 함수를 만들고 연결합니다.

```python
class Processor:
    def __init__(self, name):
        self.name = name

    def executor(self):
        ready = signal('ready')
        ready.send(self)

    def __repr__(self):
        return '<Processor %s>' % self.name


if __name__ == '__main__':
    test = Processor('test')
    test.executor()
```

Processor 객체를 만들어서 executor 함수를 수행하면 이전에 만든 ready 신호를 송신할 수 있습니다.

## data signal

```python
from blinker import signal
```

signal을 가져옵니다.

```python
send_data = signal('send-data')

@send_data.connect
def receive_data(sender, **kw):
    print(sender, kw)
```

값을 수신하여 수행할 함수를 만들 수 있습니다.

```python
result = send_data.send('anonymous', value="value")
```

send_data라는 신호를 보낼 함수에 값을 넣어서 보낼 수 있습니다.
