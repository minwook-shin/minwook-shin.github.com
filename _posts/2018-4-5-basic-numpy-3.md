---
layout: post
title: numpy 기초 실습해보기 3
---

지난 시간에 이어서 세번째로 numpy의 기초를 실습해보겠습니다.

```python
print(np.ones((2,3), dtype=float))
print(np.zeros(7, dtype=int))
```

1로 채워진 2*3의 행렬을 만듭니다.

그리고 0으로 채워진 7칸의 배열을 만듭니다.

```python
a = np.array([[1, 2, 3], [4, 5, 6]], float)
print(np.zeros_like(a))
print(np.ones_like(a))
```

2차원 배열을 만들어서 0으로 채우거나 1로 채울수 있습니다.

zeros_like와 ones_like는 이미 만들어진 곳에 채울 때 쓰입니다.

```python
print(np.identity(4, dtype=float))
print(np.eye(4,k=1, dtype=float))
```

```python
[[1. 0. 0. 0.]
 [0. 1. 0. 0.]
 [0. 0. 1. 0.]
 [0. 0. 0. 1.]]
[[0. 1. 0. 0.]
 [0. 0. 1. 0.]
 [0. 0. 0. 1.]
 [0. 0. 0. 0.]]
```

위와 같이 행렬이 만들어집니다.

```python
a = np.array([1,2,3], float)
b = np.array([5,2,6], float)
print(a+b)
print(a-b)
print(a*b)
print(b/a)
print(b**a)
```

배열끼리 연산을 할 수 있습니다.

```python
a = np.array([[1,2], [3,4]], float)
b = np.array([[2,0], [1,3]], float)
print(a * b)
```

2차원인 2*2 행렬도 가능합니다.

```python
# a = np.array([1,2,3], float)
# b = np.array([4,5], float)
# print(a + b)
```

하지만, 열이 맞지않으면 안됩니다.

```python
a = np.array([[1, 2], [3, 4], [5, 6]], float)
b = np.array([-1, 3], float)
print(a)
print(b)
print(a+b)
```

행렬의 연산을 할 수 있습니다.

```python
a = np.array([1, 4, 9], float)
print(np.sqrt(a))

a = np.array([1.1, 1.5, 1.9], float)
print(np.floor(a))
print(np.ceil(a))
print(np.rint(a))

print(np.pi)
print(np.e)
```

제곱근, 정수 내림, 정수 올림을 할 수 있습니다.

마찬가지로 numpy에서 pi와 자연상수 e값도 나타낼 수 있습니다.

> 다음 포스팅에서 계속 됩니다.