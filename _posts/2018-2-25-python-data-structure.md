---
layout: post
title: Python Data Structure 알아보기
---

오늘은 파이썬의 자료구조에 대하여 정리해보려 합니다.

## 들어가며

자료구조란 데이터를 효율적으로 관리하는 방식이며, 파이썬에서는 리스트, 튜플, 집합, 사전 등이 있습니다.

## 스택

나중에 넣은 데이터가 먼저 나오도록 설계된 구조로, 선입후출입니다.

push와 pop으로 사용할 수 있으며, 파이썬에서는 리스트를 이용하므로 ```append()```와 ```pop()```으로 구현합니다.

```python
stack = []
stack.append(10)
stack.append(20)
stack.append(30)
print(stack)
stack.pop()
stack.pop()
```

## 큐

먼저 넣은 데이터가 먼저 나오도록 설계된 구조로, 선입선출입니다.

이 역시 리스트로 이용하므로 ```append()```와 ```pop(0)```으로 구현합니다.

```python
queue = []
queue.append(10)
queue.append(20)
queue.append(30)
print(queue)
queue.pop(0)
queue.pop(0)
print(queue)
```

## 튜플

값을 변경할 수 없는 리스트입니다.

```python
t = (10,20,30)
print(t)
```

## 집합

값을 순서없이 저장하되, 중복되는 값은 없어집니다.

```python
s = set([10,20,30,30,20,10])
s.add(10)
s.remove(10)
print(s)
s.clear()
print(s)
```

## 사전

데이터 구분을 위한 고유값을 key라고 하며, 이 key를 이용하여 value를 관리하는 구조입니다.

그래서 key와 value가 매칭되면서 검색합니다.

다른 언어에서는 해시 테이블이라는 형태로 사용합니다.

```python
d = {}
d = {"fisrt" : 10,"second" : 20, "third" : 30}
print(d)
print(d.items())
print(d.keys())
print(d.values())
```

## 컬렉션

list, tuple, dict에 대한 확장된 자료구조를 제공합니다.

import collections 으로 사용할 수 있습니다.


* deque : 스택과 큐를 지원하는 모듈로서 리스트에 비해 효율적이라고 합니다.
linkedlist의 특성을 지원하며, 기존 리스트 형태의 함수도 지원합니다.

```python
import collections as c

deque_list = c.deque()
for i in range(5):
    deque_list.append(i)
print(deque_list)
for i in range(len(deque_list)):
    deque_list.rotate(i)
    print(deque_list)
print(c.deque(reversed(deque_list)))
```

* OrderedDict : 기존 사전과 달리 데이터 입력 순서대로 저장합니다.
key 또는 value를 정렬해서 사용 할 수도 있습니다.

```python
import collections as c

o = c.OrderedDict()
o["1"] = 1
o["4"] = 4
o["3"] = 3
o["2"] = 2
for k,v in o.items():
    print(k,v)
```

* defaultdict : 사전 타입의 값에 기본 값을 지정하여 새로운 값을 생성시 사용할 수 있습니다.

```python
import collections as c

d = c.defaultdict(lambda: 0) 
print(d["first"])
```

* counter : 데이터 항목들의 갯수를 사전 형태로 제공합니다.
set 연산도 지원합니다.

```python
import collections as c

count = c.Counter("hello, world!")
print(count)
```

* namedtuple : tuple 형태로 자료를 저장하는 방식이며, 저장하는 데이터의 변수를 사전에 넣을 수 있습니다.

```python
import collections as c

n = c.namedtuple("hello",["x","y"])
n2= n(11,y=12)
print(n2)
```