---
layout: post
title: numpy 기초 실습해보기 4
---

지난 시간에 이어서 네번째로 numpy의 기초를 실습해보겠습니다.

```python
a = np.array([1, 4, 5], int)
for x in a:
    print (x)

a = np.array([[1, 2], [3, 4], [5, 6]], float)
for x in a:
    print (x)

a = np.array([[1, 2], [3, 4], [5, 6]], float)
for x,y in a:
    print (x*y)
```

a의 배열을 1,4,5라고 두고 for문을 돌리면 처음부터 순서대로 수행됩니다.

그리고 행렬로 구현해도 행 단위로 출력됩니다.

```python
a = np.array([2, 4, 3], float)
print(a.sum())
print(a.prod())
print(np.sum(a))
print(np.prod(a))
```

파이썬의 함수와 numpy에서 제공하는 함수와 같은 값이 출력됩니다.

```python
a = np.array([2, 1, 9], float)
print(a.mean())
print(a.var())
print(a.std())

a = np.array([2, 1, 9], float)
print(a.min())
print(a.max())

a = np.array([2, 1, 9], float)
print(a.argmin())
print(a.argmax())
```

mean 함수로 평균을 구하거나, var 함수로 분산을 구하거나, std 함수로 표준편차를 구할 수 있으며,

min과 max로 최소 최대값을 구할 수 있습니다.

```python
a = np.array([6, 2, 5, -1, 0], float)
print(sorted(a))
a.sort()
print(a)
```

배열안의 숫자들을 정렬을 할 수 있습니다

```python
a = np.array([1, 3, 0], float)
b = np.array([0, 3, 2], float)

print(a > b)
print(a==b)
print(a<=b)

c = a > b
print(c)

a = np.array([1, 3, 0], float)
print(a>2)
```

각 열에 맞게 true와 false, 즉 boolean 값이 나옵니다.

```python
c = np.array([ True, False, False], bool)

print(any(c))
print(all(c))
```

하나라도 참이면 참으로 나오는 any 함수는 위 예시로는 true가 나오고,
모두가 참이여야지 참으로 나오는 all 함수는 위 예시로는 false가 나옵니다.

```python
a = np.array([1, 3, 0], float)
print(np.logical_and(a > 0, a < 3))

b = np.array([True, False, True], bool)
print(np.logical_not(b))

c = np.array([False, True, False], bool)
print(np.logical_or(b, c))
```

logical_and는 모든 조건에 부합하면 참, 아니면 거짓이므로 true, false, false가 나오고,
logical_not는 반대이므로 false, true, false입니다.

그리고 마지막으로 logical_or은 b와 c 배열의 같은 열중에 하나라도 true가 있다면 참입니다.

그러므로 true, true, true입니다.

> 다음 포스팅에서 계속 됩니다.