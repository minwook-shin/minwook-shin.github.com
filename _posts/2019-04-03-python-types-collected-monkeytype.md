---
layout: post
title: Python MonkeyType으로 타입 주석 생성하기
---

오늘은 Python에서 타입 힌트에 필요한 주석을 자동으로 생성해주는 MonkeyType에 대하여 알아보려 합니다.

## 개요

타입을 부여해야 할 모듈에 타입 스텁 파일을 생성하거나, 타입 주석을 코드에 작성해주는 라이브러리입니다.

## Github

https://github.com/Instagram/MonkeyType

## 라이선스

BSD 라이선스입니다.

## 설치

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
pip install MonkeyType
```

pip로 MonkeyType을 설치합니다.

## 실습

```python
def add(a, b):
    return a + b


def sub(a, b):
    return a - b


def mul(a, b):
    return a * b


def div(a, b):
    return a / b
```

우선 타입 힌트가 적용되지 않은 모듈을 하나 만들어보려 합니다.

예시로 test_module이라고 합니다.

```python
from test_module import add, sub, mul, div

if __name__ == '__main__':
    a = add(2, 1)
    b = sub(2, 1)
    c = mul(2, 1)
    d = div(2, 1)
    print(a, b, c, d)
```

test_module이라고 지은 외부 파이썬 파일에서 모든 함수를 가져옵니다.

```
monkeytype run main.py
```

외부 파이썬 파일을 쓰는 메인 함수의 파이썬 파일을 대상으로 실행하면 monkeytype.sqlite3 파일이 생성되면서 콜 트레이스를 덤프합니다.

```
monkeytype stub test_module
```

test_module에 대하여 stub 명령을 입력하면 아래와 같이 출력됩니다.

```python
def add(a: int, b: int) -> int: ...


def div(a: int, b: int) -> float: ...


def mul(a: int, b: int) -> int: ...


def sub(a: int, b: int) -> int: ...
```

변수와 함수에 타입 힌트가 적용됨을 알 수 있습니다.

```
monkeytype apply test_module
```

test_module에 대하여 apply 명령을 입력하면 아래와 같이 코드에도 적용됨을 알 수 있습니다

```python
def add(a: int, b: int) -> int:
    return a + b


def sub(a: int, b: int) -> int:
    return a - b


def mul(a: int, b: int) -> int:
    return a * b


def div(a: int, b: int) -> float:
    return a / b
```
