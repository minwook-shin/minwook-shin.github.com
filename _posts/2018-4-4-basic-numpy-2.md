---
layout: post
title: numpy 기초 실습해보기 2
---

지난 시간에 이어서 numpy의 기초를 실습해보겠습니다.

```python
a = np.array([1, 2, 3], float)
print(a.tolist())
print(list(a))
```

배열을 리스트로 변환하는 방법입니다.

```python
a = np.array([1, 2, 3], float)
s = a.tostring()
print(s)

print(np.fromstring(s))
```

tostring()과 fromstring()이 존재합니다.

```python
a = np.array([1, 2, 3], float)
print(a)
a.fill(0)
print(a)
```

fill 함수의 인자에 오는 숫자로 기존 배열을 채울 수 있습니다.

```python
a = np.array(range(6), float).reshape((2, 3))
print(a)
```

numpy로 범위를 주어서 배열을 만든 뒤, resshape로 모양을 다시 다듬었습니다.

```python
print(a.transpose())
```

transpose할 수 있습니다,

```python
a = np.array([[1, 2, 3], [4, 5, 6]], float)
print(a)
```

물론 지금까지는 일차원 배열을 했지만, 위와 같이 행렬을 만들 수 있습니다.

```
print(a.flatten())
```

1차원 배열처럼 평평하게 할 수 있습니다.

```python
a = np.array([1,2], float)
b = np.array([3,4,5,6], float)
c = np.array([7,8,9], float)
print(np.concatenate((a, b, c)))
```

각 배열을 이어서 출력할 수 있습니다.

```python
a = np.array([[1, 2], [3, 4]], float)
b = np.array([[5, 6], [7,8]], float)
print(np.concatenate((a,b)))
print(np.concatenate((a,b), axis=0))
print(np.concatenate((a,b), axis=1))
```

그냥 이을 수도 있고, axis 인자를 주어서 옆으로 이어 붙일 수 있습니다.

```python
a = np.array([1, 2, 3], float)
b = np.array([1, 2, 3], float)
print(a)
print(a[:,np.newaxis])
print(a[:,np.newaxis].shape)
print(b[np.newaxis,:])
print(b[np.newaxis,:].shape)
```

위와 같이 조절할 수 있습니다.

```python
print(np.arange(5, dtype=float))
print(np.arange(1, 6, 2, dtype=int))
```

0부터 4까지 범위가 배열을 만들어지고, 1부터 5까지 2씩 띄어가면서도 배열이 만들어 집니다.


> 다음 포스팅에서 계속 됩니다.