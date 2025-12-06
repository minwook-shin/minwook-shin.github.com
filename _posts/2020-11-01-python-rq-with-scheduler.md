---
layout: post
title: Python rq 패키지 알아보고 rq-scheduler도 사용해보기
---

오늘은 Python에서 작업을 대기열에 추가하고, 워커로 처리할 수 있는 rq 라이브러리를 알아보려 합니다.

## rq 패키지 설치

우선 virtualenv로 파이썬 환경을 분리합니다.

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

가상환경을 원하는 폴더에서 활성화합니다.

```
pip3 install --upgrade pip
```

pip의 업그레이드가 존재하는지 확인하고 진행합니다.

```
pip install rq
```

pip로 rq 패키지를 설치합니다.

```
pip install redis==3.5.3
```

해당 패키지는 Redis 기반이라서 redis 패키지를 3.0.0 보다 큰 버전으로 설치해야 합니다.

## Redis 설정

우선 redis 환경을 구축해두었다면 직접 서버를 열어도 되지만,

```
redis-server
```

도커 환경을 이용할 수도 있습니다.

```yml
version: '3.2'

services:
  redis:
    image: redis:6.0.9-alpine
    ports:
      - "6379:6379"
```

rq 패키지를 사용하는 서비스에서 해당 redis 도커 컨테이너를 연결하려면 links로 연결합니다.

```python
from redis import Redis

REDIS_HOST = 'redis'
REDIS_PORT = 6379
REDIS_DB = 0

redis_connection = Redis(host=REDIS_HOST, port=REDIS_PORT, db=REDIS_DB)
```

redis 환경이 다 구축되었다면 파이썬 코드에서 Redis 객체를 생성합니다.

## 워커 생성

대기열을 가진 워커를 생성하기 위해서 "rq worker" 명령어를 수행하거나, 도커로 별도 프로세스를 구동할 수도 있습니다.

```yml
  rq_worker:
    build:
      context: .
      dockerfile: .Dockerfile
    command: rq worker --url redis://redis:6379 main
    depends_on:
      - redis
    links:
      - redis
```

Dockerfile에서 중요한 점은 파이썬 환경에 rq 패키지가 설치되어 있어야 한다는 점입니다.

```python
queue = Queue('main', connection=redis_connection)
```

미리 생성한 redis 커넥션으로 main 워커가 담당하는 대기열을 만들어줍니다.

## 작업 생성

```python
def run_task():
    # 테스트 함수
    return {}
```

```python
job = queue.enqueue(run_task, job_id="custom_id")
```

만약 워커로 위 run_task 함수를 수행하려면 대기열에 보냅니다.

## 작업 조회

```python
job = Job.fetch(job_id, connection=redis_connection)
```

기간이 만료되지 않은 작업에 대해서는 작업에 대한 정보를 알 수 있습니다.

볼 수 있는 속성에 대한 자세한 내용은 아래에 정리했습니다.

* job.id : 작업의 고유 아이디

* job.get_status() : "finished"와 같은 작업의 상태

* job.func_name : 작업이 수행하는 함수 이름

* job.args : 작업이 수행하는 함수 인수

* job.result : 작업 결과

* job.exc_info : 작업 오류

* job.enqueued_at : 대기열에 들어간 시점

* job.started_at : 시작 시점 

* job.ended_at : 끝 시점


```python
finished_registry = FinishedJobRegistry('main', connection=redis_connection)
started_registry = StartedJobRegistry('main', connection=redis_connection)
scheduled_registry = ScheduledJobRegistry('main', connection=redis_connection)
failed_registry = FailedJobRegistry('main', connection=redis_connection)
deferred_registry = DeferredJobRegistry('main', connection=redis_connection)
```

만약 작업이 어느 상태에 들어갔는지 궁금하다면, 각 결과에 맞는 JobRegistry로 작업 목록을 가져올 수 있습니다.

```
finished_registry.get_job_ids()
```

finished 상태의 작업들을 조회하고자 하면 FinishedJobRegistry로 get_job_ids 함수를 호출합니다.

만약 result_ttl에 대해 별도로 지정하지 않았다면 500초 동안 유지되는 동안 계속 볼 수 있습니다.

## cron 표현식

cron 표현식으로 대기열 스케줄을 관리하려면 rq-scheduler 패키지를 추가로 설치해야 합니다.

우선 이전에 만들어둔 워커에 --with-scheduler 옵션을 부여합니다.

```
rqscheduler --host localhost --port 6379 --db 0
```

그리고 redis가 localhost:6379로 연결되었다면 위와 같이 rqscheduler 명령어를 이용하거나,

```yml
  scheduler:
    build:
      context: .
      dockerfile: .Dockerfile
    command: rqscheduler --host redis --port 6379 --db 0
    depends_on:
      - redis
    links:
      - redis
```

워커와 마찬가지로 스케줄러도 도커 컨테이너를 생성할 수 있습니다.

```python
scheduler = Scheduler('main', connection=redis_connection)
scheduler.cron("* * * * *", func=run_task, id="custom_id")
```

스케줄러 객체를 생성하고, cron 표현식으로 작업을 예약할 수 있습니다.

만약 1회성으로 작업 예약하려고 datetime을 사용한다면, cron이 아닌 schedule로 코드를 변경해야 합니다.

```python
jobs = scheduler.get_jobs(with_times=True)
```

대기열에 넣어진 작업은 get_jobs로 볼 수 있습니다.

```python
scheduler.cancel(job_id)
```

앞으로 특정 작업을 수행하고 싶지 않다면, 작업을 취소할 수 있습니다.


## 대시보드

웹으로 대기열을 보는 방식이 있습니다.

```
pip install rq-dashboard
```

pip로 rq-dashboard 패키지를 설치하거나,

```yml
  rq_dashboard:
    image: eoranged/rq-dashboard:latest
    ports:
      - 9181:9181
    links:
      - redis
    environment:
      - RQ_DASHBOARD_REDIS_URL=redis://redis:6379
```

이미 만들어진 도커 이미지로 컨테이너를 만들어 다른 도커 컨테이너와 같이 수행할 수 있습니다.

9181 포트에 접근하면, 웹 UI로 작업과 대기열 그리고 워커에 대한 정보를 볼 수 있습니다.