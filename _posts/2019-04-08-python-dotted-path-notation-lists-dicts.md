---
layout: post
title: Python 점 표기법으로 Dict과 List를 사용하는 dotted 패키지 알아보기
---

오늘은 Python에서의 Dict과 List를 점(.) 표기법으로 접근하는 Box 패키지에 대하여 알아보려 합니다.

## 개요

Dict과 List를 하위 경로까지 점(.) 표기법으로 접근하게 되어 복잡한 구조를 쉽게 읽고 쓸 수 있게 됩니다.

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
pip install dotted
```

pip로 dotted를 설치합니다.

## 실습

```python
from dotted.collection import DottedCollection, DottedDict, DottedList
from dotted.utils import dot, dot_json
```

dotted 패키지를 가져옵니다.

```python
dotted_arr = DottedList([[0, 1], [2, 3], [4, 5]])
```

DottedList를 만듭니다.

```python
print(dotted_arr[0])
print(dotted_arr['0.0'])

print(dotted_arr['1'])
print(dotted_arr['1.1'])

print(dotted_arr[2])
```

점(.)으로 인하여 단일 항목에 접근할 수 있습니다.

```python
dotted_arr.append(11)
print(dotted_arr)
```

```python
dotted_arr[len(dotted_arr)] = 12
print(dotted_arr)
```

기존의 DottedList에 새로운 항목을 추가할 수 있습니다.

```python
dotted_dict = DottedDict({'hello': {'world': {'python': '3'}}})
```

DottedDict을 만듭니다.

```python
print(dotted_dict['hello'])
print(dotted_dict['hello.world'])
print(dotted_dict['hello.world.python'])
```

```python
print(dotted_dict.hello)
print(dotted_dict.hello.world)
print(dotted_dict.hello.world.python)
```

기존의 키의 키를 점으로 접근할 수 있습니다.

```python
dotted_dict2 = DottedCollection.factory({
    'hello': [{'world': {'python': ['3', '7', '3']}}]
})
```

factory로 인하여 초기에 넣어지는 값에 따라 DottedDict과 DottedList이 결정됩니다.

```python
print(dotted_dict2['hello'][0]['world']['python'][0])
print(dotted_dict2.hello[0].world.python[0])
print(dotted_dict2.hello[0].world['python'][0])
print(dotted_dict2.hello[0].world['python.0'])
print(dotted_dict2.hello['0.world'].python[0])
print(dotted_dict2['hello.0.world.python.0'])
```

factory로 생성된 것도 점으로 접근할 수 있습니다.

```python
dotted_dict2['c++.path'] = {'hello': 'world'}
dotted_dict2['java.path'] = ['hello']
print(dotted_dict2)
```

dict이나 list를 새로운 항목으로 넣으면 DottedDict과 DottedList으로 추가됩니다.

```python
dot_obj = dot({'hello': 'world'})
print(dot_obj)
dotted_json = dot_json('{"hello": "world"}')
print(dotted_json)
```

dot은 DottedCollection으로 변환하고, json 문자열을 가지고도 변환할 수도 있습니다.
