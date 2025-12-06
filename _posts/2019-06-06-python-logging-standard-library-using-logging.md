---
layout: post
title: Python 로그를 남기는 logging 라이브러리 알아보기
---

오늘은 Python에 기본적으로 설치되어 있는 표준 라이브러리인 logging에 대하여 알아보려 합니다.

## logging 설치

기본적으로 logging은 설치되어 있습니다.

## basic

```python
import logging

logging.warning('warning text!')
logging.info('information text')

a = "a"
b = "b"
logging.warning('%s and %s', a, b)
```

처음 로그를 출력하면 warning만 출력됩니다.

## log file

```python
import logging

logging.basicConfig(filename='./file.log', filemode='w', level=logging.DEBUG)
logging.debug('write debug text')
logging.info('write information text')
logging.warning('write warning text')
```

파일로 로그를 저장할 수 있으며, 이 역시 저장할 레벨을 지정할 수 있습니다.

## format

```python
import logging

logging.basicConfig(
    format='%(levelname)s[%(asctime)s]:%(message)s ', level=logging.DEBUG)
logging.debug('debug text')
logging.info('information text')
logging.warning('warning text')
```

basicConfig로 출력할 로그의 포맷도 지정할 수 있습니다.

## logger

```python
import logging

logger = logging.getLogger('hello_logger')
logger.setLevel(logging.DEBUG)

ch = logging.StreamHandler()
ch.setLevel(logging.DEBUG)

ch.setFormatter(
    logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s'))

logger.addHandler(ch)

logger.debug('debug text')
logger.info('information text')
logger.warning('warning text')
logger.error('error text')
logger.critical('critical text')
```

이름 있는 Logger 객체를 만들어서 출력 레벨과 포맷들을 설정한 핸들러로 로거에 등록할 수 있습니다.

## file config

```xml
[loggers]
keys=root,loggersExample

[handlers]
keys=consoleHandler

[formatters]
keys=formattersExample

[logger_root]
level=DEBUG
handlers=consoleHandler

[logger_loggersExample]
level=DEBUG
handlers=consoleHandler
qualname=loggersExample

[handler_consoleHandler]
class=StreamHandler
level=DEBUG
formatter=formattersExample
args=(sys.stdout,)

[formatter_formattersExample]
format=[%(levelname)s]%(asctime)s - %(name)s : %(message)s
datefmt=
```

위와 같이 파일로 작성해두면 언제든지 외부에서 로거의 설정 값을 변경할 수 있습니다.

```python
import logging
import logging.config

logging.config.fileConfig('logging.conf')

logger = logging.getLogger('file config')

logger.debug('debug text')
logger.info('info text')
logger.warning('warning text')
logger.error('error text')
logger.critical('critical text')
```

불러올 설정 파일을 지정하고, 로거를 만들면 됩니다.
