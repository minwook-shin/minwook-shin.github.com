---
layout: post
title: Python 표준 datetime 전환 Delorean 라이브러리 알아보기
---

오늘은 Python의 표준 라이브러리인 datetime 모듈로 전환할 수 있는 라이브러리인 Delorean 패키지에 대하여 간단히 알아보려 합니다.

## Delorean 설치

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
pip install delorean
```

pip로 Delorean을 설치합니다.

## 예제

```python
from delorean import Delorean
from delorean import parse
from datetime import timedelta
```

Delorean와 datetime을 가져옵니다.

```python
d = Delorean()
print(d)
```

Delorean 객체로 현재 시각의 datetime과 utc에 대한 정보를 만듭니다.

```python
ds = d.shift("Asia/Seoul")
print(ds)
```

현재 시간대에서 (Delorean 객체와 연관된) 지정된 시간대로 시간대를 이동합니다.

Delorean 객체가 변경되어 반환됩니다.

```python
print(ds.next_sunday())
```

다음 sunday에 맞는 날짜가 반환됩니다.

```python
print(ds.replace(hour=12))
```

Delorean 객체에 있는 datetime의 시간을 변경합니다.

```python
print(ds.truncate('day'))
```

Delorean 객체에 있는 datetime의 단위를 자릅니다.

위 코드와 같은 경우에는 일자까지만 자릅니다.

```python
print(ds.date)
```

date 객체를 반환합니다.

```python
print(ds.datetime)
print(type(ds.datetime))
```

datetime을 반환합니다.

타입을 확인해도 datetime.datetime로 출력됩니다.

```python
print(ds.naive)
print(ds.epoch)
```

naive와 epoch에 대한 datetime을 가져옵니다.

```python
d = Delorean()
d + timedelta(hours=2)
d - timedelta(hours=2)
```

날짜의 간격을 나타내는 timedelta에 의해 시간을 추가하고 뺄 수 있습니다.

```python
dp = parse("2019/05/02 00:00:00 -0700")
print(dp.date)
```

우리가 읽을 수 있는 문자열을 파싱할 수도 있습니다.
