---
layout: post
title: Python logging 알아보기
---

오늘은 파이썬에서 logging을 알아보려합니다.

logging은 프로그램이 실행되는 동안 일어나는 정보를 기록을 남기는 것입니다.

유저의 접근, 프로그램의 예외처리, 특정 함수등을 콘솔 화면에 출력하거나 파일에 남깁니다.

실행 시점에서 남겨야하거나 개발 시점에서 남겨야하는 기록에 필요합니다.

## print 차이점

기록을 print로 남기는 것도 가능하지만, 콘솔 창에만 남기는 기록은 분석시 사용불가합니다.

레벨별로 기록을 남길 필요가 있고, 모듋별로 별도의 기록도 남길 필요가 있습니다.

이를 지원하는 모듈이 logging입니다.

## 레벨

프로그램 진행 상황에 따라 서로 다른 레벨의 기록을 출력합니다.

info, debug, warning, error, critical이 있습니다.

* debug : 변수를 호출하고 변경할 때처럼 개발시 처리기록을 남겨야되는 로그 정보

* info : 사용자가 프로그램에 접속할 때처럼 처리가 진행되는 정보

* warning : 사용자가 잘못 입력한 정보나 처리를 할 때 알림 정보

* error : 잘못된 처리로 인한 사소한 오류 정보

* critical : 사용자의 강제 종료처럼 잘못된 처리로 프로그램이 동작하지 못할 때 정보

## 예시 코드 

```python
import logging

log = logging.getLogger("main")

stream = logging.StreamHandler()

log.addHandler(stream)

log.setLevel(logging.DEBUG)
log.debug("디버그")
log.info("안내")
log.warning("주의")
log.error("오류")
log.critical("치명적")

print("-"*10)

log.setLevel(logging.INFO)
log.debug("디버그")
log.info("안내")
log.warning("주의")
log.error("오류")
log.critical("치명적")

print("-"*10)

log.setLevel(logging.WARNING)
log.debug("디버그")
log.info("안내")
log.warning("주의")
log.error("오류")
log.critical("치명적")

print("-"*10)

log.setLevel(logging.ERROR)
log.debug("디버그")
log.info("안내")
log.warning("주의")
log.error("오류")
log.critical("치명적")

print("-"*10)

log.setLevel(logging.CRITICAL)
log.debug("디버그")
log.info("안내")
log.warning("주의")
log.error("오류")
log.critical("치명적")

print("-"*10)
```

logging 모듈을 임포트하고, Logger 선언합니다.
그리고 Logger의 출력방식을 선언하고, 핸들러에 등록합니다.
마지막으로 레벨을 지정하는 setLevel 메소드로 레벨을 조정합니다.

레벨에 따라서 기록이 보이는 범위가 다르게 보입니다.

참고로 이렇게 작성하면 IDE의 디버그 창에 그대로 출력됩니다.

## Configparser

프로그램의 실행 설정을 파일에 저장합니다.

설정파일을 사전 형태로 호출하고 사용하게 됩니다.

```python
[section]
Status: nomal
```

configparser 역시 임포트해서 사용하며, 사전 형태로 불러와집니다.

## Argparser

콘솔 창에서 프로그램 실행시 설정 값을 저장합니다.

콘솔 기반 파이썬 프로그램에서 대부분 지원합니다.

명령행 옵셥이라고도 부른다고 합니다.

```python
argparser.add_argument('-t',"--test",dest="test arg",help="A integers arg", type=int)
```

이렇게 짧은 이름과 긴 이름, 그리고 설명과 인자 타입을 작성하여 추가할 수 있습니다.

## formmater

logging의 결과값의 형식을 지정할 수도 있습니다.

```python
f = logging.Formatter("%(levelname)s %(message)s")
```

오류가 나올때 원하는 정보를 원하는 위치에 지정할 수 있다는 겁니다.

