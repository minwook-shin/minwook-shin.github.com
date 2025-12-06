---
layout: post
title: Python 브라우저로 보는 시각화 Bokeh 라이브러리 알아보기
---

오늘은 Python을 가지고 브라우저에서 데이터를 시각화할 수 있는 Bokeh 패키지에 대하여 알아보려 합니다.

## Bokeh 설치

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
pip install bokeh
```

pip로 Bokeh를 설치합니다.

## example

```python
from bokeh.plotting import figure, output_file, show
```

bokeh를 가져옵니다.

```python
x = [1, 2, 3, 4, 5]
y = [1, 2, 3, 4, 5]
```

x축과 y축을 정의합니다.

```python
output_file("hello.html")
```

hello.html이라는 파일에 그래프를 그릴 준비를 합니다.

```python
p = figure(title="example", x_axis_label='x', y_axis_label='y')
```

example이라는 제목을 가진 figure 객체를 만듭니다.

```python
p.line(x, y)
```

선을 그립니다.

```python
show(p)
```

figure 객체를 가지고 그래프를 그립니다.

## figure / gridplot

```python
import numpy as np
```

sin, cos, tan을 그리기 위해서 numpy를 가져옵니다.

```python
from bokeh.layouts import gridplot
from bokeh.plotting import figure, output_file, show
```

bokeh를 가져옵니다.

```python
x = np.linspace(0, 4*np.pi, 100)
```

지정된 간격으로 공백을 반환하는 linspace를 사용하여 x축을 정의합니다.

```python
y0 = np.sin(x)
y1 = np.cos(x)
```

sin, cos, tan을 정의합니다.

```python
output_file("lines.html")
```

lines.html이라는 파일에 그래프를 그릴 준비를 합니다.

```python
s1 = figure()
s1.circle(x, y0)
```

figure 객체를 생성하고, 그 안에 x축과 y축으로 sin을 그립니다.

```python
s2 = figure()
s2.triangle(x, y1)
```

figure 객체를 생성하고, 그 안에 x축과 y축으로 cos을 그립니다.

```python
p = gridplot([[s1, s2]])
```

캔퍼스에 plot의 그리드를 만듭니다.

```python
show(p)
```

figure 객체를 가지고 그래프를 그립니다.
