---
layout: post
title: Python 환율 계산 Forex 라이브러리 알아보기
---

오늘은 Python에서 환율을 계산할 수 있는 라이브러리인 Forex 패키지에 대하여 간단히 알아보려 합니다.

## Forex 설치

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
pip install forex-python
```

pip로 Forex를 설치합니다.

## 환율 예제

```python
import datetime
from forex_python.converter import CurrencyRates
from forex_python.converter import CurrencyCodes
```

forex_python과 datetime을 가져옵니다.

```python
c = CurrencyRates()
usd = c.get_rates('USD')
print(usd)
krw = c.get_rates('KRW')
print(krw)
```

CurrencyRates 객체로 각 화폐 단위에 따른 환율을 구할 수 있습니다.

```python
c = CurrencyRates()
usd_krw = c.get_rates('USD')['KRW']
print(usd_krw)
krw_usd = c.get_rates('KRW')['USD']
print(krw_usd)
```

딕셔너리의 형태로 되어 있기 때문에 각각의 환율에 접근할 수 있습니다.

```python
date = datetime.datetime(2018, 5, 5, 18, 0, 0, 151012)
c = CurrencyRates()
print(c.get_rates('USD', date)['KRW'])
```

특정 날짜의 환율을 확인할 수 있습니다.

```python
c = CurrencyRates()
usd_to_krw = c.get_rate('USD', 'KRW')
print(usd_to_krw)
```

딕셔너리처럼 접근하는 것처럼 인자를 추가하여 알아낼 수 있습니다.

```python
date = datetime.datetime(2018, 5, 5, 18, 0, 0, 151012)
c = CurrencyRates()
print(c.get_rate('USD','KRW', date))
```

특정 날짜의 지정한 화폐의 환율도 알 수 있습니다.

```python
c = CurrencyRates()
con = c.convert('USD', 'KRW', 100)
print(con)
```

100달러에 대한 원화를 환전한 금액을 출력할 수 있습니다.

```python
date = datetime.datetime(2018, 5, 5, 18, 0, 0, 151012)
c = CurrencyRates()
print(c.convert('USD','KRW',100, date))
```

이전의 특정 날짜에 대해 100달러를 원화로 환전한 금액을 출력할 수 있습니다.

```python
c = CurrencyCodes()
sb = c.get_symbol('KRW')
print(sb)
```

원화의 기호를 출력할 수 있습니다.
