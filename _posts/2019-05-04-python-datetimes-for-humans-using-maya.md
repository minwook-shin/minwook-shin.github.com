---
layout: post
title: Python datetime 개선한 Maya 라이브러리 알아보기
---

오늘은 Python의 표준 라이브러리인 datetime 모듈을 사람이 읽기 좋게 포팅한 라이브러리인 Maya 패키지에 대하여 간단히 알아보려 합니다.

## Maya 설치

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
pip install maya
```

pip로 Maya를 설치합니다.

## 예제

```python
import maya
import time
from datetime import datetime
```

maya와 time, 그리고 datetime을 가져옵니다.

```python
now = maya.now()
print(now)
```

MayaDT 객체로 현재 epoch 시간을 가져옵니다.

```python
tomorrow = maya.when('tomorrow')
print(tomorrow)
```

사람이 부르는 단어로 시간를 출력합니다.

위 코드는 현재 시각의 다음날이 반환됩니다.

```python
ds = tomorrow.slang_date()
print(ds)
```

tomorrow로 출력할 수 있습니다.

```python
st = tomorrow.slang_time()
print(st)
```

in 23 hours로 출력할 수 있습니다.

```python
iso = tomorrow.iso8601()
print(iso)
```

내일 날짜 기준으로 ISO 8601로 반환합니다.

```python
rtc2822 = tomorrow.rfc2822()
print(rtc2822)
```

내일 날짜 기준으로 RFC 2822로 반환합니다.

```python
rtc3339 = tomorrow.rfc3339()
print(rtc3339)
```

내일 날짜 기준으로 RFC 3339로 반환합니다.

```python
dt = tomorrow.datetime()
print(dt)
print(type(dt))
```

내일 날짜 기준으로 datetime.datetime 타입으로 반환됩니다.

```python
pd = maya.parse('2019-05-04 18:23:45.423992+00:00').datetime(to_timezone="Asia/Seoul", naive=True)
print(pd)
```

문자열로 이루어져 있는 날짜와 시간을 파싱하여 타임존을 설정할 수 있습니다.

```python
fd = maya.MayaDT.from_datetime(datetime.utcnow())
print(fd)
```

datetime 객체를 MayaDT 객체로 만들 수 있습니다.

```python
fs = maya.MayaDT.from_struct(time.gmtime())
print(fs)
```

튜플 구조체를 MayaDT 객체로 만들 수 있습니다.

```python
md = maya.MayaDT(time.time())
print(md)
```

실수형으로도 날짜를 구할 수 있습니다.

```python
when = maya.when('2019-05-04', timezone="Asia/Seoul")
print(when)
print(when.day)
print(when.add(days=5).day)
print(when.timezone)
```

날짜나 시간대와 같은 정보를 datetime 처럼 필드로 접근할 수 있습니다.

```python
interval = maya.intervals(start=maya.now(), end=maya.now().add(days=1), interval=60 * 60)
print(interval)
for i in interval:
    print(i)
```

지정된 간격과 시작하거나 끝나는 시간을 가진 MayaDT 객체를 iterator처럼 사용할 수 있습니다.

```python
snap = maya.when('Mon, 06 May 2019 21:21:42 GMT').snap('@d+3h').rfc2822()
print(snap)
```

snaptime 패키지가 포함되어 있어서 시간을 고칠 수 있습니다.
