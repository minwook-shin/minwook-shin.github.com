---
layout: post
title: Python DataClass 데코레이터 알아보기
---

오늘은 Python에서 데이터 클래스를 구성하게 해주는 DataClass 데코레이터에 대하여 알아보려 합니다.

## 개요

데이터를 저장하기 위한 클래스를 데이터 클래스라고 하며, 이를 파이썬 3.7부터 데코레이터로 지원합니다.

## pep

https://www.python.org/dev/peps/pep-0557/

## 매개 변수

dataclass 데코레이터의 매개 변수는 아래와 같이 기본적으로 설정되어 있습니다.

```python
@dataclass(init=True, repr=True, eq=True, order=False, unsafe_hash=False, frozen=False)
```

- init: \_\_init\_\_ 메서드

기본적으로 \_\_init\_\_ 메서드가 생성됩니다.

- repr: \_\_repr\_\_ 메서드

기본적으로 \_\_repr\_\_ 메서드가 생성됩니다.

- eq: \_\_eq\_\_ 메서드

기본적으로 \_\_eq\_\_ 메서드가 생성됩니다.

- order: \_\_lt\_\_, \_\_le\_\_, \_\_gt\_\_, \_\_ge\_\_ 메서드

기본적으로 생성되지 않습니다.

- unsafe_hash: \_\_hash\_\_ 메서드

기본적으로 생성되지 않습니다.

- frozen: 읽기 전용 고정 인스턴스

기본적으로는 필드에 값을 넣어도 됩니다.

## 기존

```python
class User:
    def __init__(self):
        self.name = ""
        self.agree = True
        self.tag = []
```

```python
    def __getitem__(self, idx):
        return self.tag[idx]
```

기존에는 \_\_init\_\_ 메소드로 필드를 구성했습니다.

## 데이터클래스 데코레이터

```python
from dataclasses import dataclass, field, asdict, astuple, make_dataclass
```

dataclasses 패키지를 가져옵니다.

```python
from typing import List
```

typing 패키지로 리스트를 타입 힌트에 활용하게 합니다.

```python
@dataclass
class User:
    name: str = ""
    agree: bool = True
    tag: List[str] = field(default_factory=list)
```

User라는 데이터 클래스를 만듭니다.

해당 코드에서는 name, agree, tag 필드로 이루어져 있습니다.

가변적인 기본값을 가진 리스트는 field로 대입합니다.

```python
p = User("Kim", True)
```

User 데이터 클래스로 객체를 생성했습니다.

```python
print(asdict(p))
assert asdict(p) == {'name': "Kim", 'agree': True,'tag': []}
```

dict 타입으로 변경할 수 있습니다.

```python
print(type(p))
print(type(asdict(p)))
```

타입을 비교해보면 \_\_main\_\_.User과 dict임을 알 수 있습니다.

```python
print(astuple(p))
assert astuple(p) == ("Kim", True, [])
```

tuple 타입으로 변경할 수 있습니다.

```python
print(type(p))
print(type(astuple(p)))
```

타입을 비교해보면 \_\_main\_\_.User과 tuple임을 알 수 있습니다.

```python
Article = make_dataclass('article',
                   [('title', str),
                    ('text', str),
                    ('index', int, field(default=5))])
```

클래스 이름과 필드를 순서대로 적으면 데이터 클래스를 만들 수 있습니다.

```python
a = Article("title","text",1)
print(a)
print(asdict(a))
print(astuple(a))
```

Article 데이터 클래스로 객체를 생성하고 dict 타입 또는 tuple 타입으로 변경할 수 있습니다.
