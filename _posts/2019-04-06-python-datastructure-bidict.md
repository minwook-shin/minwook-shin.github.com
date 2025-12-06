---
layout: post
title: Python bidict 사용하기
---

오늘은 Python에서 양방향 dict인 bidict 패키지에 대하여 알아보려 합니다.

## 개요

bidict는 키와 값을 서로 찾기 편하게 되어 있는 양방향 dict입니다.

그렇기 때문에 키는 물론이고 값도 유니크해야 합니다.

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
pip install bidict
```

pip로 bidict를 설치합니다.

## 실습

```python
from bidict import bidict, IGNORE
```

bidict 패키지를 가져옵니다.

```python
hello_world = bidict({'hello': 'world'})
```

기본적으로 bidict 객체는 키와 값을 넣어서 만듭니다.

```python
symbol = hello_world['hello']
print(symbol)
inverse_symbol = hello_world.inverse['world']
print(inverse_symbol)
```

키로 값을 검색할 수 있고, 값으로 키를 검색할 수 있습니다.

```python
print(hello_world.inverse)
```

키와 값을 역으로 돌릴 수도 있습니다.

```python
test_value = bidict({'First': 'one', 'Second': 'Two', 'Third': 'Three'})
test_value['First'] = 'One'
```

인덱스로 검색해서 값을 변경할 수 있습니다.

```python
print(sorted(test_value.keys()))
print(sorted(test_value.values()))
```

keys는 키만 정렬되고, values는 값만 정렬되어 반환됩니다.

```python
print(test_value)
del test_value.inverse['Three']
print(test_value)
```

del 키워드로 원하는 인덱스를 제거할 수 있습니다.

위 코드에서는 inverse로 값을 검색해서 Third라는 키의 쌍을 지웠습니다.

```python
print('First' in test_value)
```

collections.abc.MutableMapping을 지원해서 파이썬의 문법을 자연스럽게 접목할 수 있습니다.

```python
print(test_value.get('First', 'default'))
```

get으로 첫번째 인자의 문자열을 키로 검색해서 있으면 반환하고, 없으면 두번째 인자를 기본 값으로 반환합니다.

```python
print(test_value.pop('Second', 'default'))
print(test_value)
```

pop은 get과 같이 값을 반환하지만, 꺼내면서 반환하기 때문에 지워집니다.

```python
test_value.update(new_key="new_value")
print(test_value)
```

키와 값을 업데이트할 수 있습니다.

```python
# test_value.update(new_key2="new_value")
# print(test_value)
test_value.forceput('new_key2','new_value')
print(test_value)
```

강제로 업데이트 과정을 수행하여 같은 키나 값이 있다면 덮어씌어버립니다.

주석 처리되어 있는 코드는 이미 있는 값 때문에 bidict.\_exc.ValueDuplicationError 오류가 출력됩니다.

```python
test_value.putall([('new_key', 'new_value')], on_dup_val=IGNORE)
print(test_value)
```

키와 값의 중복을 막으려면 on_dup_val 옵션에 IGNORE을 주면 됩니다.

```python
print(bidict(hello='world') == dict(hello='world'))
```

다형성으로 인해 bidict과 dict은 같은 키와 값이라면 같다고 인식됩니다.
