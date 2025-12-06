---
layout: post
title: Python 2D 플로팅 Matplotlib 라이브러리 알아보기
---

오늘은 Python을 가지고 2D 플로팅할 수 있는 Matplotlib 패키지에 대하여 알아보려 합니다.

## Matplotlib 설치

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
pip3 install --upgrade pip
```

pip의 업그레이드가 존재하는지 확인하고 진행합니다.

```
pip install matplotlib
```

pip로 Matplotlib를 설치합니다.

## figure

```python
import matplotlib.pyplot as plt
```

matplotlib을 가져옵니다.

```python
fig = plt.figure()
```

새로운 figure 객체를 생성합니다.

```python
fig.suptitle('No figure')
```

figure 중심에 제목을 추가할 수 있습니다.

```python
fig, ax_lst = plt.subplots(2, 2)
```

subplots을 설정합니다.

```python
plt.show()
```

figure 객체를 가지고 그림을 출력합니다.

## line

```python
import matplotlib.pyplot as plt
```

matplotlib를 가져옵니다.

```python
plt.plot([1, 2, 3, 4])
```

x축 값으로 라인 플롯을 그립니다.

```python
plt.xlabel('x label')
plt.ylabel('y label')
```

x축과 y축의 라벨을 작성합니다.

```python
plt.title("Plot")
```

제목을 작성합니다.

```python
plt.legend(['example'])
```

범례를 추가합니다.

```python
plt.show()
```

figure 객체를 가지고 그림을 출력합니다.

최종적으로 범례와 그래프가 나타나게 됩니다.

## sin

```python
import matplotlib.pyplot as plt
import numpy as np
```

matplotlib와 numpy를 가져옵니다.

```python
x = np.arange(0, 10, 0.2)
y = np.sin(x)
```

증가되는 수열을 생성하고, sin 곡선을 만들 준비도 합니다.

```python
plt.plot(x, y)
```

x축 값(증가 수열)과 y축 값(sin)으로 라인 플롯을 그립니다.

```python
plt.show()
```

figure 객체를 가지고 그림을 출력합니다.

최종적으로 sin 곡선이 그려지게 됩니다.
