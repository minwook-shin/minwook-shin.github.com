---
layout: post
title: Python Box 사용하기
---

오늘은 Python에서 점(.) 표기법으로 접근하는 딕셔너리인 Box 패키지에 대하여 알아보려 합니다.

## 개요

Box는 점 표기법으로 접근해서 데이터를 읽고 쓰는 dict입니다.

덕 타이핑으로 인해 오리와 같이 보이고, 운다면 오리라고 생각하는 것처럼 딕셔너리와 호환됩니다.

box 객체는 to_dict()으로 인해 딕셔너리로 전환할 수 있습니다.

## bidict 설치

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
pip install python-box
```

pip로 box를 설치합니다.

## 실습

```python
from box import Box
```

box 패키지를 가져옵니다.

```python
user_data = {
    "users": {
        "Kim": {
            "id": "realKimID",
            "email": "Kim@example.com",
            "friend": [{"name": "Lee", "status": "activated"},
                       {"name": "ahn", "status": "activated"}]
        },
        "Shin": {
            "id": "realShinID",
            "email": "Shin@example.com",
            "friend": [{"name": "Lee", "status": "deactivated"},
                       {"name": "ahn", "status": "deactivated"}]
        },
    }
}

user_box = Box(user_data)
print(user_box.users.Kim.email)
```

dict 타입을 box 객체로 만듭니다.

점 표기법으로 접근하면 하위 데이터를 참조할 수 있습니다.

users.Kim.email이라면 Kim@example.com가 반환됩니다.

```python
print(user_box.users.Kim.friend[0])
print(user_box.users.Kim.friend[-1])
```

하위 데이터가 리스트인 경우에는 인덱스로도 접근할 수 있습니다.

```python
user_box.users.Kim.friend.append({"name": "han", "status": "activated"})
```

하위 데이터가 리스트인 경우에는 추가하여 새로 Box를 반환합니다.

```python
print(user_box.users.Kim.to_dict())
print(user_box.users.Kim == user_box.users.Kim.to_dict())
```

to_dict()은 인하여 일반적인 dict으로 변환할 수 있습니다.

하지만, 덕 타이핑으로 인해 같다고 인식됩니다.

```python
test = Box({'hello': 10}, world=10)
print(test)
```

dict와 동일한 방법으로 Box 객체를 만들 수 있습니다.

```python
frozen_box = Box(data={'frozen': 'box',}, frozen_box=True)
# frozen_box.frozen = 'Box'
# box.BoxError: Box is frozen
```

frozen_box로 값을 바꾸려면 BoxError가 생깁니다.

```python
empty_box = Box(default_box=True)

print(empty_box.a.b.c)
empty_box.a.b.c.d = "h"
print(empty_box)
```

default_box로 생성하면 값을 여러 단계로 내려가서 대입할 수 있습니다.

```python
camel_killer_box = Box(HelloWorld="HELLO, WORLD!", camel_killer_box=True)
print(camel_killer_box.hello_world)
```

camel_killer_box는 snake case로 검색되도록 할 수 있습니다.

```python
box_of_order = Box(ordered_box=True)
box_of_order.c = 1
box_of_order.a = 2
box_of_order.d = 3
print(box_of_order.keys() == ['c', 'a', 'd'])
```

ordered_box는 키가 입력된 순서를 유지시킵니다.

```python
from box import BoxList
box_list = BoxList({'item': x} for x in range(10))
print(box_list[5].item)
```

iterable한 변수를 넣어 연속적인 BoxList를 만들 수 있습니다.

```python
from box import SBox
s_box = SBox(hello='world')
print(s_box.json)
```

일반적인 box에서 dict으로 변환하는 것보다 빠르게 json으로 변환할 수 있습니다.
