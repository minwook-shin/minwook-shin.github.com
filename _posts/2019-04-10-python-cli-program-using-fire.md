---
layout: post
title: Python fire 패키지를 사용하여 cli 프로그램 만들기
---

오늘은 Python에서의 모든 객체를 command line interface로 만들어 주는 fire 패키지에 대하여 알아보려 합니다.

## 개요

원래의 파이썬 함수들을 command line interface로 만들어서 터미널과 통합된 환경을 만들 수 있습니다.

네이밍의 이유는 "fire"이라고 불리면 명령을 수행하기 때문이라고 소개되고 있습니다.

## fire 설치

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
pip install fire
```

pip로 fire를 설치합니다.

## 기본

```python
import fire


def hello(name):
    return 'Hello {name}!'.format(name=name)


if __name__ == '__main__':
    fire.Fire()
```

python3 main.py hello python 라고 하면 hello 함수에 python 문자열이 들어갑니다.

## 함수

```python
import fire


def hello(name):
    return 'Hello {name}!'.format(name=name)


if __name__ == '__main__':
    fire.Fire(hello)
```

hello라는 함수가 이미 Fire에 등록되어 있으므로 python3 main.py python 라고 하면 함수가 실행됩니다.

## 클래스

```python
import fire


class Main(object):

    def __init__(self, text):
        self.text = text


if __name__ == '__main__':
    fire.Fire(Main)
```

클래스를 등록하면 클래스 속성에 접근할 수 있습니다.

python3 main.py --text hello text 라고 하면 hello라는 문자열이 등록된 클래스의 속성에 접근합니다.

## 딕셔너리

```python
import fire


def add(x, y):
    return x + y


def sub(x, y):
    return x - y


if __name__ == '__main__':
    fire.Fire({
        'add': add,
        'subtract': sub,
    })
```

터미널에 입력할 때에 명령을 고칠 수 있습니다.

python3 calculator2/main.py subtract 20 10 라고 하면 딕셔너리로 등록된 값인 sub 함수가 실행됩니다.

## 클래스

```python
import fire


class Calculator(object):

    @classmethod
    def add(cls, x, y):
        return x + y

    @classmethod
    def sub(cls, x, y):
        return x - y


if __name__ == '__main__':
    fire.Fire(Calculator)
```

python3 calculator3/main.py sub 20 10 라고 하면 클래스의 sub 메소드가 수행됩니다.

## 그룹화

```python
import fire


class Car1(object):

    @classmethod
    def run(cls):
        return 'running car1'


class Car2(object):

    @classmethod
    def run(cls):
        return "running car2"


class Cli(object):

    def __init__(self):
        self.car1 = Car1()
        self.car2 = Car2()

    def run(self):
        self.car1.run()
        self.car2.run()


if __name__ == '__main__':
    fire.Fire(Cli)
```

여러 클래스들을 묶어줘서 명령을 그룹화합니다.

python3 main.py car1 run 라고 하면 특정 클래스의 메소드가 수행됩니다.
