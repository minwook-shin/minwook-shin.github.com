---
layout: post
title: Python 로그를 남기는 Logbook 라이브러리 알아보기
---

오늘은 Python으로 로그를 남길 수 있는 Logbook를 알아보려 합니다.

## Logbook 설치

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
pip install Logbook
```

pip로 Logbook을 설치합니다.

## stream handler

```python
import sys
from logbook import Logger, StreamHandler
```

형식화된 로깅 레코드를 스트림에 쓰는 StreamHandler를 사용할 수 있습니다.

sys.stdout 이나 sys.stderr를 사용할 수 있습니다.

```python
log = Logger('Stream handler logger')
```

Logger 객체를 생성합니다.

```python
StreamHandler(sys.stdout).push_application()
```

컨텍스트 객체를 응용 프로그램 스택에 푸시합니다.

```python
log.warn('warning')
log.error("error")
```

로그 객체로 warning과 error 레벨의 로그를 남길 수 있습니다.

## stderr handler

```python
from logbook import StderrHandler
```

stderr에 있는 것을 기록하는 핸들러인 StderrHandler를 사용할 수 있습니다.

```python
handler = StderrHandler()
```

StderrHandler 객체를 생성합니다.

```python
handler.format_string = '{record.channel}: {record.message}'
handler.push_application()
```

출력형식을 지정하고, 컨텍스트 객체를 응용 프로그램 스택에 푸시합니다.

```python
log.warn('warning')
log.error("error")
```

로그 객체로 warning과 error 레벨의 로그를 남길 수 있습니다.

## file handler

```python
import logbook
```

logbook을 가져옵니다.

```python
only_debug = logbook.FileHandler('./debug.log', filter=lambda r, h: r.extra['debug'])
```

```python
everything = logbook.FileHandler('./all.log', bubble=True)
```

파일을 열고 닫는 핸들러를 만들 수 있습니다.

람다로 특정 로그만 필터링할 수도 있습니다.

```python
with only_debug, everything:
    logbook.info('this is debug code', extra={'debug': True})
    logbook.info('this is not debug code')
```

로그를 출력해보면 FileHandler에 의해 파일로 저장됩니다.
