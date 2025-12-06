---
layout: post
title: Python 스케줄러를 만드는 apscheduler 라이브러리 알아보기
---

오늘은 Python으로 특정 날짜, 특정 간격으로 반복적인 스케줄러를 만들 수 있는 apscheduler를 사용해보려 합니다.

## apscheduler 설치

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
pip install apscheduler
```

pip로 apscheduler를 설치합니다.

## text

```python
from apscheduler.schedulers.blocking import BlockingScheduler
```

BlockingScheduler를 가져옵니다.

```python
scheduler = BlockingScheduler()
scheduler.add_job('sys:stdout.write', 'interval', seconds=5, args=['hello,world!\n'])
```

단일 스케줄러를 만들고 특정 명령과 인자, 그리고 interval에 5초로 설정합니다.

5초 간격으로 표준출력으로 hello,world!가 출력되게 됩니다.

interval외에도 date, cron도 존재합니다.

```python
sched.add_job('sys:stdout.write', 'date', run_date=date(2019, 6, 5), args=['hello,world!\n'])
```

만약 date로 설정한 경우에는 run_date를 설정해주어야 합니다.

```python
try:
    scheduler.start()
except KeyboardInterrupt:
    pass
```

스케줄러를 시작하며, ctrl+c으로 종료할 때의 예외처리도 진행합니다.

## blocking

```python
from datetime import datetime
from apscheduler.schedulers.blocking import BlockingScheduler
```

BlockingScheduler를 가져옵니다.

```python
def hello_world():
    print('hello, world! time : %s' % datetime.now())
```

hello_world 함수를 만들어서 호출될 때마다 문자열과 날짜가 출력되게 합니다.

```python
scheduler = BlockingScheduler()
scheduler.add_job(hello_world, 'interval', seconds=5)
```

BlockingScheduler로 특정 함수를 5초마다 수행할 수 있습니다.

```python
try:
    scheduler.start()
except KeyboardInterrupt:
    pass
```

스케줄러를 시작하며, ctrl+c으로 종료할 때의 예외처리도 진행합니다.

## backgroud

```python
import time
from datetime import datetime
from apscheduler.schedulers.background import BackgroundScheduler
```

BackgroundScheduler를 가져옵니다.

```python
def hello_world():
    print('hello, world! time : %s' % datetime.now())
```

hello_world 함수를 만들어서 호출될 때마다 문자열과 날짜가 출력되게 합니다.

```python
scheduler = BackgroundScheduler()
scheduler.add_job(hello_world, 'interval', seconds=5)
```

BackgroundScheduler로 특정 함수를 5초마다 수행할 수 있습니다.

```python
scheduler.start()
time.sleep(10)
```

백그라운드에서 스케줄러를 시작하고, 10초를 쉬고 종료합니다.

time.sleep이 없다면 백그라운드 상에서만 구동되므로 실제 파일은 바로 종료됩니다.

## asyncio

```python
from datetime import datetime
from apscheduler.schedulers.asyncio import AsyncIOScheduler
import asyncio
```

asyncio으로 작성된 코드에서도 스케줄러를 사용할 수 있습니다.

```python
def hello_world():
    print('hello world! time : %s' % datetime.now())
```

마찬가지로 hello_world 함수를 만들어서 호출될 때마다 문자열과 날짜가 출력되게 합니다.

```python
scheduler = AsyncIOScheduler()
scheduler.add_job(hello_world, 'interval', seconds=5)
scheduler.start()
```

AsyncIOScheduler를 만들고 함수를 등록한 뒤에 시작합니다.

```python
try:
    asyncio.get_event_loop().run_forever()
except KeyboardInterrupt:
    pass
```

이벤트 루프를 수행하며, ctrl+c으로 종료할 때의 예외처리도 진행합니다.
