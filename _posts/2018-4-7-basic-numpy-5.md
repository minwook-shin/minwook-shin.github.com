---
layout: post
title: numpy 기초 실습해보기 5
---

지난 시간에 이어서 다섯번째로 numpy의 기초를 실습해보겠습니다.

```python
a = np.array([1, 3, 0], float)
print(np.where(a != 0, 1 / a, a))
print(np.where(a > 0, 3, 2))
```

where 함수는 어떤 조건에 따라 경우에 맞게 보여줍니다.

```python
a = np.array([[0, 1], [3, 0]], float)
print(a.nonzero())
```

행렬에서 0이 아닌 부분은 1로 출력되고 나머지는 0로 출력됩니다.

```python
a = np.array([1, np.NaN, np.Inf], float)
print(a)
print(np.isnan(a))
print(np.isfinite(a))
```

isnan은 not a number인지 판별해주고, isfinite는 무한대인지 판별해줍니다.

```python
[ 1. nan inf]
[False  True False]
[ True False False]
```

첫번째 식을 판별한 결과, 위와 같이 출력됩니다.

```python
a = np.array([[6, 4], [5, 9]], float)
print(a >= 6)
print(a[a >= 6])

a = np.array([[6, 4], [5, 9]], float)
sel = (a >= 6)
print(a[sel])

a[np.logical_and(a > 5, a < 9)]
print(np.array([ 6.]))
```

배열에 조건문을 작성하여 형태를 유지하면서 각각의 boolean 값을 출력할 수 있습니다.

또한 식에 부합하는 것만 모아서 리스트로 모을 수 있습니다.

```python
a = np.array([[0, 1], [2, 3]], float)
b = np.array([2, 3], float)
c = np.array([[1, 1], [4, 0]], float)
print(a)

print(np.dot(b, a))
print(np.dot(a, b))
print(np.dot(a, c))
print(np.dot(c, a))
```

dot 함수는 두 인자의 행렬 곱을 나타냅니다.

```python
a = np.array([1, 4, 0], float)
b = np.array([2, 2, 1], float)

print(np.outer(a, b))
print(np.inner(a, b))
print(np.cross(a, b))
```

outer 함수는 두 벡터의 외적을 계산하는 함수이며, 
inner 함수는 일반적인 내적을 곱한 것의 합하는 함수입니다.

마지막으로 cross 함수는 1차원 벡터곱 연산을 cross 연산할 수 있습니다.

```python
a = np.array([[4, 2, 0], [9, 3, 7], [1, 2, 1]], float)
print(a)
print(np.linalg.det(a))
```

numpy 패키지의 linalg 서브 패키지에는 det 함수가 있습니다.

이를 이용하여 행렬식을 계산할 수 있습니다.

```python
vals, vecs = np.linalg.eig(a)
print(vals)
print(vecs)
```

numpy 패키지의 linalg 서브 패키지에는 eig 함수도 있습니다.
백터의 고윳값을 찾을 수 있습니다.

> 다음 포스팅에서 계속 됩니다.