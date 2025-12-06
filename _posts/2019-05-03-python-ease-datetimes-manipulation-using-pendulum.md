---
layout: post
title: Python datetime 쉽게 조작하는 Pendulum 라이브러리 알아보기
---

오늘은 Python의 표준 라이브러리인 datetime 모듈을 쉽게 조작할 수 있다는 라이브러리인 Pendulum 패키지에 대하여 간단히 알아보려 합니다.

## Pendulum 설치

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
pip install pendulum
```

pip로 Pendulum을 설치합니다.

## 예제

```python
import pendulum
```

pendulum을 가져옵니다.

```python
dt = pendulum.datetime(2019, 5, 1)
print(type(dt))
```

년도, 월, 일을 가지고 datetime 객체를 만듭니다.

```python
pendulum.datetime(2019, 5, 1, tz="Asia/Seoul")
```

시간대도 설정할 수 있습니다.

```python
dt = pendulum.local(2019, 5, 1)
print(dt.timezone.name)
```

로컬 타임존에 맞게 설정할 수 있습니다.

```python
now = pendulum.now()
print(now)
```

현제 시간을 알 수 있습니다.

```python
naive = pendulum.naive(2019, 5, 1)
print(naive.timezone)
```

naive DateTime을 알 수 있습니다.

```python
dt = pendulum.parse('2019-05-01T22:00:00')
print(dt)
```

날짜와 시간이 적힌 문자열을 파싱할 수 있습니다.

```python
pendulum.set_locale('ko')
print(pendulum.now().add(years=1).diff_for_humans())
```

언어를 설정하면 이후에 생성되는 DateTime 객체는 한국어로 출력됩니다.

위 코드는 "1년 후"로 출력됩니다.

```python
dt = pendulum.parse('2019-05-01T22:00:00')

print(dt.year)
print(dt.month)
print(dt.day)
print(dt.hour)
print(dt.minute)
print(dt.second)
print(dt.microsecond)
print(dt.day_of_week)
print(dt.day_of_year)
print(dt.week_of_month)
print(dt.week_of_year)
print(dt.days_in_month)
```

세부적인 필드로 접근할 수 있습니다.

```python
dt = pendulum.now()
print(dt.set(year=2017, month=12, day=22).to_datetime_string())
```

객체를 날짜와 시간의 형식을 지키게 반환할 수 있습니다.

```python
pendulum.now()
print(dt.to_date_string())
```

날짜만 형식을 지키게 반환할 수 있습니다.

```python
dt = pendulum.datetime(2019, 5, 1, 00, 00, 00)
print(dt.format('YYYY-MM-DD HH:mm:ss'))
print(dt.format('[today] dddd'))
```

주어진 형식대로 출력할 수 있습니다.

```python
dt = pendulum.datetime(2019, 5, 1, 1, 1, 1)
print(dt.start_of('day'))
```

일자를 기준으로 잘라서 시간을 재설정할 수 있습니다.
