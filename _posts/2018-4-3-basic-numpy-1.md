---
layout: post
title: numpy 기초 실습해보기 1
---

오늘은 numpy로 간단한 예제를 실행하면서 알아보겠습니다.

```python
import numpy
import numpy as np 
```

우선 numpy를 설치되었다는 가정하에 numpy를 import할 수 있습니다.

as로 줄여서 쓸 준비도 할 수 있습니다.

```python
a = np.array([1, 4, 5, 8], float)
print(a)
```

array를 float 형태로 만듭니다.
아직 일차원 배열입니다.

```python
print(type(a))
```

타입을 조회해보면 'numpy.ndarray'로 나오는 것을 알 수 있습니다.

```python
print(a[:2])
```

문자열 자르듯이 array도 자를 수 있습니다.

```python
print(a[3])
```

물론 특정 인덱스를 가져올 수도 있습니다.

이제 본격적으로 행렬을 만들어 보았습니다.

```python
a = np.array([[1, 2, 3], [4, 5, 6]], float)
```

이러면 2차원 배열로 보이는 행렬이 나왔습니다.

```python
a = np.array([[1, 2, 3], [4, 5, 6]], float)
print(a)
print(a[0,0])
print(a[0,1])

a = np.array([[1, 2, 3], [4, 5, 6]], float)
print(a[1,:])
print(a[:,2])
print(a[-1:,-2:])
```

이 역시 위에 했던 문자열 자르기나 인덱스가 가능합니다.

```python
print(a.shape)
```

shape로 차원의 크기를 튜플로 알 수 있습니다.

```python
a = np.array([[1, 2 ,3 ],[4, 5, 6]],float)
print(len(a))
```

길이를 조회하면 행의 길이가 나옵니다.

```python
a = np.array(range(5), float)
```

```python
b = np.arange(5, dtype=float)
```

이렇게하면, 둘 다 같은 결과가 나옵니다.

```python
a = a.reshape((5,2))
print(a)
print(a.shape)
```

위에 알아본 shape 함수와 연관지어서, 이 reshape는 차원의 크기를 설정할 수 있는 함수입니다.

```python
a = np.array([1, 2, 3], float)
b = a
c = a.copy()
a[0] = 0
print(a)
print(b)
print(c)
```

a 와 b, c를 만들어서 b는 그냥 a에서 대입하고, c는 복사 함수를 이용했습니다.

copy() 함수를 써야지 완전히 개별적인 행렬이 됩니다.

> 다음 포스팅에서 계속 됩니다.