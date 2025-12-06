---
layout: post
title: Python 통계 그래프 Seaborn 라이브러리 알아보기
---

오늘은 Python을 가지고 matplotlib에 기반한 통계 그래프를 그리는 Seaborn 패키지에 대하여 알아보려 합니다.

## seaborn 설치

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
pip install seaborn
```

pip로 seaborn을 설치합니다.

## statistical relationships

```python
import matplotlib.pyplot as plt
import seaborn as sns
```

seaborn과 출력할 figure를 나타내기 위한 matplotlib 패키지를 가져옵니다.

```python
sns.set()
```

seaborn 기본 스타일로 설정하여 미적 매개 변수를 설정합니다.

```python
tips = sns.load_dataset("tips")
sns.relplot(x="total_bill", y="tip", data=tips)
```

tips 데이터셋을 불러오고, relplot으로 통계 관계를 나타냅니다.

x축과 y축, 그리고 데이터 셋을 설정합니다.

```python
plt.show()
```

figure를 출력합니다.

## categorical data

```python
import seaborn as sns
import matplotlib.pyplot as plt
```

seaborn과 출력할 figure를 나타내기 위한 matplotlib 패키지를 가져옵니다.

```python
sns.set()
```

seaborn 기본 스타일로 설정하여 미적 매개 변수를 설정합니다.

```python
tips = sns.load_dataset("tips")
sns.catplot(x="day", y="total_bill", data=tips)
```

tips 데이터셋을 불러오고, catplot으로 분포를 나타냅니다.

x축과 y축, 그리고 데이터 셋을 설정합니다.

```python
plt.show()
```

figure를 출력합니다.

## distribution of a dataset

```python
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
```

seaborn과 출력할 figure를 나타내기 위한 matplotlib 패키지를 가져옵니다.

그리고 numpy를 가져옵니다.

```python
sns.set()
```

seaborn 기본 스타일로 설정하여 미적 매개 변수를 설정합니다.

```python
x = np.random.normal(size=100)
sns.distplot(x)
```

numpy로 무작위 샘플을 그립니다.

그리고 distplot으로 관측 분포를 나타냅니다.

```python
plt.show()
```

figure를 출력합니다.

## linear relationships

```python
import seaborn as sns
import matplotlib.pyplot as plt
```

seaborn과 출력할 figure를 나타내기 위한 matplotlib 패키지를 가져옵니다.

```python
sns.set()
```

seaborn 기본 스타일로 설정하여 미적 매개 변수를 설정합니다.

```python
tips = sns.load_dataset("tips")
sns.regplot(x="total_bill", y="tip", data=tips)
```

tips 데이터셋을 불러오고, regplot으로 선형 관계를 나타냅니다.

x축과 y축, 그리고 데이터 셋을 설정합니다.

```python
plt.show()
```

figure를 출력합니다.
