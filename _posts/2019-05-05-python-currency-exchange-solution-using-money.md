---
layout: post
title: Python 통화 교환 솔루션 Money 라이브러리 알아보기
---

오늘은 Python에서 통화 교환 솔루션 라이브러리인 Money 패키지에 대하여 간단히 알아보려 합니다.

## Money 설치

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
pip install money
```

pip로 Money를 설치합니다.

## 돈 계산

```python
from money import Money
```

money를 가져옵니다.

```python
m = Money(2000, currency='KRW')
print(m.currency)
print(m.amount)
print(m)
```

통화를 지정하고 화폐 금액을 인자로 넣어 Money 객체를 생성할 수 있습니다.

```python
m = Money(2000, currency='KRW')
print(m/2)
print(m*2)
print(m+1000)
print(m-1000)
```

숫자로 화폐의 단위를 추가하거나 뺴거나 곱하거나 나누면서 조작할 수 있습니다.

```python
m = Money(2000, currency='KRW')
m2 = Money(4000, currency='KRW')
print(m*2 == m2)
```

서로 값이 같은지 비교할 수 있습니다.

```python
class KRW(Money):
    def __init__(self, amount='0'):
        super().__init__(amount=amount, currency='KRW')


price = KRW('10000')
print(price)
```

Money의 하위 클래스를 만들어서 통화 프리셋을 만들 수 있습니다.
위와 같은 경우에는 KRW라는 하위 클래스로 통화 단위를 설정한 모습입니다.

## 환전

서로 다른 환율을 가진 화폐를 환전할 수 있습니다.

```python
from decimal import Decimal
from money import Money, xrates
```

money와 decimal을 가져옵니다.

```python
xrates.install('money.exchange.SimpleBackend')
xrates.base = 'USD'
xrates.setrate('USD', Decimal('1'))
xrates.setrate('KRW', Decimal('1165.95'))
```

현재 환율과 매우 유사하게 설정해보았습니다.

```python
usd = Money(100, 'USD')
krw = Money(1, 'KRW')
```

100달러와 1원을 준비해보았습니다.

```python
print(usd.to('KRW'))
print(krw.to('USD'))
print(usd.to('KRW') + krw)
```

달러를 원화로 바꾸어보면 환율이 적용되어 있음을 확인할 수 있습니다.
