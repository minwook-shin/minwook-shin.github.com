---
layout: post
title: Python 국제화할 수 있는 Babel 라이브러리 알아보기
---

오늘은 Python으로 국제화(l18n)을 제공할 수 있는 babel를 사용해보려 합니다.

## Babel 설치

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
pip install Babel
```

pip로 Babel을 설치합니다.

## locale

pybabel --list-locales

기본적으로 적용할 수 있는 로케일을 터미널에서 확인해볼 수 있습니다.

```python
from babel import Locale
```

Locale을 가져옵니다.

```python
locale = Locale('en', 'US')
print(locale.territories['US'])

locale = Locale('ko', 'KR')
print(locale.territories['US'])
```

각 나라의 이름을 특정 로케일에 맞게 출력할 수 있습니다.

```python
lang = Locale.parse('ko_KR')
print(lang.display_name)
print(lang.english_name)
print(lang.language)
print(lang.language_name)
```

로케일의 이름과 영문 이름을 볼 수 있습니다.

우리나라 기준으로
display_name은 한국어 (대한민국), english_name은 Korean (South Korea), language는 ko, language_name은 한국어로 출력됩니다.

```python
locale = Locale('ko')
month_names = locale.months['format']['wide'].items()
for _, name in sorted(month_names):
    print(name)
```

```python
month_names = locale.days['format']['wide'].items()
for _, name in sorted(month_names):
    print(name)
```

특정 로케일로 몇 월인지, 몇 요일인지 알 수 있습니다.

## date

```python
from datetime import date
from babel.dates import format_date
```

format_date와 datetime의 date를 추가적으로 가져옵니다.

```python
d = date(2019, 6, 1)
print(format_date(d, locale='ko'))
print(format_date(d, locale='ko_KR'))
```

특정 날짜를 특정 로케일에 맞게 설정할 수 있습니다.

```python
print(format_date(d, format='short', locale='ko'))
print(format_date(d, format='long', locale='ko'))
print(format_date(d, format='full', locale='ko'))
```

포맷을 설정할 수 있습니다.

long부터는 2019년 6월 1일과 같이 출력됩니다.

```python
from datetime import timedelta
from babel.dates import format_timedelta
```

timedelta와 format_timedelta를 가져옵니다.

```python
delta = timedelta(days=6)
print(format_timedelta(delta, locale='ko_KR'))
```

특정 로케일로 맞게 기간을 출력할 수 있습니다.

ko_KR로 설정했으므로 1주라고 출력됩니다.

## number

```python
from babel.numbers import format_decimal
```

format_decimal를 가져옵니다.

```python
print(format_decimal(1234567890, locale='en_US'))
print(format_decimal(1234567890, locale='ko_KR'))
```

각 로케일에 맞게 포맷을 맞춥니다.

공통적으로 1,234,567,890으로 출력됩니다.

## gettext

```python
import gettext
```

gettext를 가져옵니다.

```python
_ = gettext.gettext
print(_('translatable'))
```

해당 문자열로 번역할 준비를 합니다.

```
pybabel extract . -o base.pot
```

터미널에서 pybabel로 파이썬 파일에 gettext로 이루어진 문자열을 가져와 base.pot을 생성합니다.

```
pybabel init -l ko_KR -i base.pot -d lang
```

base.pot을 기반으로 한국어에 대한 번역파일을 lang이라는 폴더의 하위에 생성합니다.

```
pybabel compile -d lang
```

컴파일을 통해 mo 파일을 생성할 수 있습니다.
