---
layout: post
title: python 타입 힌트와 typing 패키지를 알아보고, mypy로 타입 체크하기
---

오늘은 python 3.6에 도입된 타입 힌트를 알아보고, 이에 같이 접목할 수 있는 typing 패키지도 같이 사용해보려고 합니다. 그리고 타입 힌트를 검사해주는 mypy 패키지도 설치해서 작성한 코드의 타입을 잘 작성했는지 검사해볼 것 입니다.

## 타입 힌트

타입 힌트는 예상치 못한 타입이 변수에 할당되는 것을 pycharm과 같은 ide나 mypy와 같은 검사기가 막아주는 것이 주목적입니다.

3.5 버전부터 도입되기 시작 타입 힌트는 3.6버전에 와서 변수에도 적용가능하도록 추가되었습니다.

## typing

```python
from typing import *
```

typing 패키지를 가져옵니다.

보통은 import를 전부 가져오는 것이 아니라 사용하는 것들만 추가해야되지만, 이번 포스팅에서는 별표로 전부 사용하겠다고 지정합니다.

```python
text: str = "hello, world!"
number: int = 10
```

문자열과 정수는 간단하게 표기할 수 있습니다.

```python
def func1(url: str) -> str:
    return "hello, world!"
```

함수에서도 인자의 타입과 반환 값의 타입을 지정할 수 있습니다.

위는 문자열로 받아서 문자열로 반환하는 함수입니다.

```python
def func2(url: str) -> None:
    print("hello, world!")
```

print와 같이 반환 값이 None인 경우에는 위와 같이 할 수 있습니다.

```python
Vector = List[float]
```

List도 typing 패키지로 타입을 표기할 수 있습니다.

```python
dictionary = Dict[str, int]
tupleArr = Tuple[str, int]
```

Dict과 Tuple도 List와 같이 타입을 표기할 수 있습니다.

```python
UserId = NewType('UserId', int)
id = UserId(10)
```

정수로 쓰이는 타입을 별칭으로 만들어줄 수 있씁니다.

```python
class custom(NamedTuple):
    id: int
    name: str

user = custom(0, "user")
```

Tuple의 확장으로 NamedTuple이 있습니다.

```python
def callable(next_item: Callable[[], str]) -> None:
    pass
```

callable도 타입으로 지정할 수 있습니다.

```python
T = TypeVar('T')

def GenericFunc(l: Sequence[T]) -> T:
    return l[0]
```

Generic을 TypeVar로 구현할 수 있습니다.

```python
def vec2(x: T, y: T) -> List[T]:
    return [x, y]
```

위와 같이 Generic을 사용할 수 있습니다.

```python
a = None
b = Any

def foo(bar: Any) -> int:
    return 1
```

None은 무엇이든지 올 수 있으며, Any도 모든 타입을 받을 수 있습니다.

```python
data = ""
def legacy(text: Any) -> Any:
    return data
```

그래서 옛날 코드들은 위와 같이 Any로 처리하면 됩니다.

```python
def unionFunc(p: Union[int, float]) -> float:
    return p
```

특정한 여러 타입을 자유롭게 사용하려면 Union 타입을 사용합니다.

## mypy

ide 없이 타입을 올바로 사용했는지 확인해주는 검사해줍니다.

```
pip3 install mypy
```

mypy를 pip3로 설치합니다.

```
mypy file.py
```

mypy로 파이썬 파일을 지정하고 수행하면 특정 줄에 타입이 잘 못 지정되었다고 출력됩니다.
