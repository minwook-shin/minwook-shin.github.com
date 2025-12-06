---
layout: post
title: numpy 기초 실습해보기 6
---

지난 시간에 이어 여섯번째 시간으로 numpy의 기초를 실습해보려합니다.

```python
b = np.linalg.inv(a)

print(b)
print(np.dot(a, b))
```

numpy 패키지의 linalg 서브 패키지에는 inv 함수라는 역행렬 구하는 함수가 있습니다.

```python
a = np.array([[1, 3, 4], [5, 2, 3]], float)
U, s, Vh = np.linalg.svd(a)
print(U)
print(s)
print(Vh)
```

numpy 패키지의 linalg 서브 패키지에 존재하는 svd 함수를 사용하여 3개의 반환값(U,s,Vh)을 처리합니다.

각 반환 값을 출력합니다.

```python
print(np.poly([-1, 1, 1, 10]))
print(np.roots([1, 4, -2, 3]))
print(np.polyder([1./4., 1./3., 1./2., 1., 0.]))
print(np.polyval([1, -2, 0, 2], 4))
```

poly 함수로는 방정식의 계수를 구합니다.

roots 함수로는 다항식의 해를 구합니다.

polyder 함수로 다항식의 미분을 구합니다.

polyval 함수는 스칼라 값에서 다항식의 해를 구합니다.

```
x = [1, 2, 3, 4, 5, 6, 7, 8]
y = [0, 2, 1, 3, 7, 10, 11, 19]
print(np.polyfit(x, y, 2))
```

polyfit 함수는 입력과 출력값으로 다항식의 계수를 찾아줍니다.
첫번째로 들어오는 인자는 다항식의 입력값이며, 두번째로 들어오는 인자는 다항식의 결과입니다.

마지막 인자는 차수의 다항식 계수 값입니다.

```
a = np.array([1, 4, 3, 8, 9, 2, 3], float)
print(np.median(a))
```

지정된 축을 따라 median 값을 계산하며, 배열의 값을 반환합니다.

```
a = np.array([[1, 2, 1, 3], [5, 3, 1, 8]], float)
c = np.corrcoef(a)
print(c)
print(np.cov(a))
```

corrcoef 함수는 피어슨 상관계수를 구하는 함수입니다.

cov 함수로는 공분산을 계산합니다.

```
np.random.seed(293423)
print(np.random.rand(5))
print(np.random.rand(2,3))
print(np.random.rand(6).reshape((2,3)))
print(np.random.random())
print(np.random.randint(5, 10))
```

난수를 생성하는 시드를 설정해줄 수 있습니다.

그리고 행렬도 만들 수 있습니다.

처음부터 행과 열을 지정하여 만들 수 있으며, reshape 함수로 다시 지정할 수도 있습니다.

```python
print(np.random.normal(1.5, 4.0))
print(np.random.normal())
print(np.random.normal(size=5))
```

normal 함수로 정규분포를 띄게할 수 있습니다.

```python
l = list(range(10))
print(l)
np.random.shuffle(l)
print(l)
```

리스트로 만들어서 numpy 패키지의 random 서브 패키지에서 shuffle 함수를 쓰면 주어진 수들을 섞을 수 있습니다.

 > 끝