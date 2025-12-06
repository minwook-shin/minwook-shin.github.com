---
layout: post
title: Python 선언적 통계 시각화 Altair 라이브러리 알아보기
---

오늘은 Python을 가지고 선언적으로 통계를 시각화할 수 있는 Altair 패키지에 대하여 알아보려 합니다.

## Altair 설치

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
pip install Altair
```

pip로 Altair를 설치합니다.

## 그림 파일로 내보내기

png 그림 파일로 내보내려면 selenium과 브라우저 드라이버가 필요합니다.

만약 selenium이 설치되어 있지 않다면 아래와 같이 출력됩니다.

ImportError: selenium package is required for saving chart as png

```
pip install selenium
```

selenium을 설치하고 실행해도 chromedriver가 없다면 다시 오류가 발생합니다.

Message: 'chromedriver' executable needs to be in PATH.

```
brew tap homebrew/cask
```

```
brew cask install chromedriver
```

mac의 경우에는 chromedriver를 brew로 쉽게 설치할 수 있습니다.

## 데이터 셋

간단한 예제를 위해서 pandas에서 간단한 데이터 셋을 가져옵니다.

```python
pip install vega_datasets
```

```python
import pandas as pd

data = pd.DataFrame({
    'x': ['a', 'b', 'c', 'd', 'e', 'f'],
    'y': [11, 22, 33, 44, 55, 66, 77, 88, 99]
})
```

위와 같은 pandas DataFrame 기반의 데이터 셋입니다.

## example

```python
import altair as alt
```

altair를 가져옵니다.

```python
from vega_datasets import data
```

pandas DataFrame 기반의 데이터 셋을 가져옵니다.

```python
print(data.cars())
```

출력해보면 406개의 rows와 9개의 columns가 존재합니다.

```python
cars = data.cars()
```

```python
chart = alt.Chart(cars).mark_point().encode(
    x='Horsepower',
    y='Miles_per_Gallon',
    color='Origin',
)
```

점으로 이루어진 산포도를 그릴 수 있습니다.

만약 막대 그래프로 그리고 싶다면 mark_bar()로 바꾸면 됩니다.

x와 y는 column을 지정해주면 됩니다.

```python
chart.save('chart.png')
```

이전에 설치한 selenium과 브라우저 드라이버로 그림 파일을 내보내 확인할 수 있습니다.
