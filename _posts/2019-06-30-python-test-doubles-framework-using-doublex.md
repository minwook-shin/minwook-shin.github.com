---
layout: post
title: Python 테스트 더블 doublex 라이브러리 알아보기
---

오늘은 Python에서 가상의 객체를 만들어서 테스트할 수 있는 테스트 더블의 doublex 라이브러리를 알아보려 합니다.

## doublex 설치

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
pip install doublex
```

pip로 doublex를 설치합니다.

## stub

원하는 반환 값을 가질 수 있게 합니다.

```python
from doublex import Stub, ANY_ARG
```

doublex를 가져옵니다.

```python
class Collaborator:
    def except_method(self):
        pass

    def add_method(self, a, b):
        pass
```

테스트할 클래스를 만듭니다.

```python
with Stub(Collaborator) as stub:
    stub.except_method().raises(KeyError)
    stub.add_method(ANY_ARG).returns(4)
```

해당 클래스의 stub을 생성하고 메소드들의 반환 값을 정해줍니다.

```python
print(stub.add_method(2, 3))
```

해당 stub은 무조건 4가 반환됩니다.

```python
try:
    stub.except_method()
except KeyError:
    print("key error!")
```

해당 stub은 KeyError가 발생합니다.

## free stub

```python
from doublex import Stub
```

doublex를 가져옵니다.

```python
with Stub() as stub:
    stub.example('test').returns(10)

result = stub.example('test')
print(result)
```

클래스 없이도 stub을 생성해서 특정 반환 값으로 지정할 수 있습니다.

## spy

Spy는 Stub을 확장하여 생성 후 호출에 대해 전달할 수 있습니다.

```python
from doublex import Spy
```

doublex를 가져옵니다.

```python
class Sender:
    def send_method(self, address, force=True):
        pass
```

클래스를 만듭니다.

```python
sender = Spy(Sender)
sender.send_method("example@example.net")
```

Spy객체를 만들고 값을 넣습니다.

```python
try:
    sender.send_method()
except TypeError:
    print("argument error")
```

테스트할 메소드는 인자가 존재하므로 인자없이 수행하면 오류가 발생합니다.

## mock

```python
from doublex import Mock

with Mock() as mock:
    mock.test().returns(10)

print(mock.test())
```

mock을 만들고 특정 메소드의 반환 값을 지정할 수 있습니다.