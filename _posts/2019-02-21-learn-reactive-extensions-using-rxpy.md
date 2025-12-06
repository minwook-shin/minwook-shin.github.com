---
layout: post
title: Reactive Extensions 알아보고 RxPY 실습해보기
---

오늘은 비동기 프로그래밍을 구현할 수 있는 Reactive Extensions인 Rx에 대하여 알아보고, RxPY도 설치하고 실습해보려 합니다.

Rx는 observer pattern을 확장하여 데이터와 이벤트 시퀀스를 지원하며, 비동기 시퀀스르 지원하기 위한 Observable를 중심으로 비동기 프로그래밍을 하게 됩니다.

## 왜 Observable를 이용하는가

공식 홈페이지에서 나온대로,

```
The ReactiveX Observable model allows you to treat streams of asynchronous events with the same sort of simple, composable operations that you use for collections of data items like arrays.
```

Observable은 기존의 배열을 사용해도 비동기 이벤트의 스트림을 처리할 수 있습니다.

## Observable 방식

옵저버가 Observable을 구독하고, Observable이 발생시키는 이벤트를 옵저버가 발견하고 순서대로 처리합니다.

Observable이 발생시키는 동안 다른 이벤트를 기다릴 필요가 없기 때문에 옵저버에서는 동시 작업이 가능합니다.

간단하게 작성하자면, 옵저버가 Observable을 구독하면 Observable는 옵저버의 메소드를 호출하게 되는 방식입니다.

## Observable 메소드

- 데이터 검색

다음에 사용할 데이터가 있다고 알려줍니다.

```java
onNext(T)
```

- 오류 탐색

오류가 발생했다고 알려줍니다.

```java
onError(Exception)
```

- 완료

더 이상 사용할 데이터가 없다고 알려줍니다.

```java
onCompleted()
```

- 구독

Subscribe 메서드로 Observable을 관찰합니다.

```python
observable = method()
observable.subscribe(OnNext, Error, Complete)
```

## 연산자

[공식 홈페이지의 문서](http://reactivex.io/documentation/operators.html)에 따르면 Observables를 생성하거나, 변형/결합할 때 혹은 필터링할 때에 필요한 연산자들이 있습니다.

## Observables 생성

새로운 Observables를 만들어주는 연산자입니다.

- Create

- Defer
- Empty/Never/Throw
- From
- Interval
- Just
- Range
- Repeat
- Start
- Timer

## Observables 변형

Observable에 의해 전송되는 데이터의 변형을 해주는 연산자입니다.

- Buffer

- FlatMap
- GroupBy
- Map
- Scan
- Window

## Observables 필터링

일부만 선택해서 Observable에 의해 데이터가 전송되는 연산자입니다.

- Debounce

- Distinct
- ElementAt
- Filter
- First
- IgnoreElements
- Last
- Sample
- Skip
- SkipLast
- Take
- TakeLast

## Observables 결합

하나의 Observable를 만들기 위해서 여러 소스를 결합해주는 연산자입니다.

- And/Then/When

- CombineLatest
- Join
- Merge
- StartWith
- Switch
- Zip

## 오류 처리 연산자

Observable에서 오류를 처리해주는 연산자입니다.

- Catch

- Retry

## RxPY로 Reactive Extensions 경험하기

RxPY는 비동기 프로그래밍을 구현할 수 있는 Reactive Extensions의 python 라이브러리입니다.

이번 포스팅에서는 python이 설치하기 쉽고, 코드의 가독성이 좋기 때문에 RxPY로 비동기 프로그래밍을 경험해보려 합니다.

## RxPY 설치

```
pip3 install rx
```

rx-1.6.1이 설치되며,

현재 최신 버전인 3.0으로 설치하려면

```
pip3 install -pre rx
```

pre 옵션을 주어야 합니다.

저 같은 경우에는 python3로 설치하려고 pip3에서 진행했습니다.

## RxPY와 관련된 pylint 오류 수정

제 개인적인 문제인지도 모르겠으나, vscode 에디터에서 python 확장을 설치하게 되면 pylint를 사용하게 되어 rx 관련 맴버가 없다고 경고를 띄우게 됩니다.

프로젝트 최상단 폴더에 .pylintrc 파일을 새로 저장한 뒤에

```json
generated-members=rx.*
```

위 구문을 추가해주시면 됩니다.

## RxPY 작성 예제

```python
from rx import Observable, Observer
```

rx 패키지를 가져와서 Observable과 Observer를 사용합니다.

```python
class PrintObserver(Observer):

    def on_next(self, value):
        print(value)

    def on_completed(self):
        print("Done")

    def on_error(self, error):
        print(error)
```

자신만의 Observer 클래스를 구성하여 Observer를 상속받습니다.

```python
source = Observable.from_(["python", "java", "kotlin", "go"])
source.subscribe(PrintObserver())
```

from 연산자로 리스트를 Observable하게 바꿔줍니다.

새로운 Observable로 변환하면 그 자체가 소스로 지정되어 옵저버가 구독하게 됩니다.

옵저버는 위에서 구독한 Observer 클래스입니다.

수행하게 되면 python, java, kotlin, go가 순서대로 on_next 메소드를 거치게 되면서 출력되게 됩니다.
마지막으로 go라고 출력되면 더 이상 사용할 데이터가 없으므로 on_completed 메소드를 거쳐서 마무리되게 됩니다.
