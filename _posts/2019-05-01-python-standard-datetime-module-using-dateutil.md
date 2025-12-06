---
layout: post
title: Python 표준 datetime 모듈 확장 dateutil 라이브러리 알아보기
---

오늘은 Python의 표준 라이브러리인 datetime 모듈의 확장 라이브러리인 dateutil 패키지에 대하여 간단히 알아보려 합니다.

델타 시간을 계산하거나, 두 시간대의 차이를 나타낼 수도 있습니다.

## dateutil 설치

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
pip install dateutil
```

pip로 dateutil을 설치합니다.

## 예제

```python
from dateutil.relativedelta import *
```

기존 datetime 모듈에 호환되므로 datetime의 특정 구성 요소를 대체하거나 시간 간격을 나타내도록 dateutil.relativedelta를 가져옵니다.

```python
from dateutil.easter import *
```

GM Arts에서 작성한 특정 연도의 일반적인 부활절 계산 방법을 위한 dateutil.easter를 가져옵니다.

```python
from dateutil.rrule import *
```

결과 캐싱에 대한 지원과 같이 iCalendar RFC에 반복 규칙을 구현할 수 있는 dateutil.rrule을 가져옵니다.

```python
from dateutil.parser import *
```

문자열로된 날짜를 파싱할 수 있는 dateutil.parser를 가져옵니다.

```python
now = parse("Wed May 01 01:01:50 UTC 2019")
print(now)
```

문자열을 파싱하여 시간대를 가져옵니다.

위 코드는 2019-05-01 01:01:50+00:00으로 출력됩니다.

```python
today = now.date()
print(today)
```

파싱한 시간대를 기반으로 날짜만 잘라서 가져옵니다.

위 코드는 2019-05-01으로 출력됩니다.

```python
count = list(rrule(DAILY, count=10, dtstart=parse("20190501T090000")))
print(count)
```

rrule에 의해 10번 카운트될 때까지 계속 특정 날짜부터 1일 증가하게 됩니다.

출력될 때에는 리스트로 변환되어 출력됩니다.

```python
year = rrule(YEARLY, dtstart=now, bymonth=6, bymonthday=23, byweekday=FR)[0].year
relative_delta = relativedelta(easter(year), today)
print(relative_delta)
print(today+relative_delta)
```

relativedelta로 시간 간격을 대체하거나 나타낼 수 있습니다.

위 코드로 알 수 있는 점은 해당 날짜로 부터 years=+3, months=+11, days=+8되어야
2023년 4월 9일이 다가올 수 있다는 점입니다.
