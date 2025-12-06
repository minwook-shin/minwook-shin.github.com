---
layout: post
title: Python datetime mocking하는 freezegun 라이브러리 알아보기
---

오늘은 Python에서 가상의 시간을 현재 시간으로 적용해서 테스트할 수 있는 freezegun 라이브러리를 알아보려 합니다.

## freezegun 설치

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
pip install freezegun
```

pip로 freezegun을 설치합니다.

## 예제

```python
from freezegun import freeze_time
import datetime
```

freezegun과 datetime을 가져옵니다.

```python
@freeze_time("2017-12-22")
def test():
    return datetime.datetime.now()


print(test())
```

데코레이터로 함수를 감싸고 2017년 12월 22일이 현재 시간이라고 고정할 수 있습니다.

테스트하면 2017-12-22 00:00:00 라고 출력됩니다.

```python
@freeze_time("2017-12-22")
class Tester(object):
    def test_the_class(self):
        return datetime.datetime.now()


print(Tester().test_the_class())
```

데코레이터로 클래스를 감싸고 2017년 12월 22일이 현재 시간이라고 고정할 수 있습니다.

테스트하면 2017-12-22 00:00:00 라고 출력됩니다.

```python
@freeze_time("Jan 1th, 2018")
def test_human():
    return datetime.datetime.now()


test_human()
```

데코레이터로 할 때에 위와 같은 형식으로 해도 인식합니다.

```python
print(datetime.datetime.now())
with freeze_time("2017-12-22"):
    print(datetime.datetime.now())
print(datetime.datetime.now())
```

with으로 특정 범위에서만 날짜를 지정할 수 있습니다.

```python
freezer = freeze_time("2017-12-22 12:00:01")
freezer.start()
print(datetime.datetime.now())
freezer.stop()
```

날짜를 고정할 타이밍을 변동할 수도 있습니다.

```python
initial_datetime = datetime.datetime(year=1, month=1, day=1,
                                     hour=1, minute=1, second=1)
other_datetime = datetime.datetime(year=2, month=2, day=2,
                                   hour=2, minute=2, second=2)
with freeze_time(initial_datetime) as frozen_datetime:
    print(frozen_datetime())
    frozen_datetime.move_to(other_datetime)
    print(frozen_datetime())
```

```python
@freeze_time("2017-12-22", as_arg=True)
def test(frozen_time):
    print(datetime.datetime.now())
    frozen_time.move_to("2018-01-01")
    print(datetime.datetime.now())


test()
```

특정 날짜로 옯겨서 멈출 수 있습니다.
