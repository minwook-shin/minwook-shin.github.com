---
layout: post
title: RxPY Observable 조합하는 방법에 대하여 알아보기
---

오늘은 RxPY에서 Observable을 조합해주는 방법들을 실습해보려 합니다.

RxPY는 Reactive Extensions의 python 라이브러리입니다.

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

## RxPY creation

```python
from rx import *
```

rxpy를 사용하기 위해서 rx 패키지를 가져옵니다.

```python
class ObserverClass(Observer):

    def on_next(self, value):
        print("print : " + str(value))

    def on_completed(self):
        print("Done!")

    def on_error(self, error):
        print("Error : "+str(error))
```

Observer를 상속 받은 옵저버 클래스를 만들어줍니다.

여기에는 on_next, on_error, on_completed 메소드로 구성됩니다.

이를 이용해서 생성한 Observable을 옵저버 클래스에 구독해줍니다.

```python
fisrt = Observable.range(1, 5)
second = Observable.just(20)
fisrt.merge(second).subscribe(ObserverClass())
```

merge는 두 Observable들을 병합하여 나열해줍니다.

```python
Observable.repeat(Observable.from_((1, 2, 3)),
                  3).merge_all().subscribe(ObserverClass())
```

merge_all은 Observable들을 한 Observable로 병합합니다.

```python
ob1 = Observable.from_((1, 2))
ob2 = Observable.from_((3, 4))
Observable.concat([ob2, ob1]).subscribe(ObserverClass())
```

concat은 첫 Observable이 끝나면 다음 Observable을 이어 붙입니다.

```python
obRange = Observable.range(0, 6)
Observable.zip(obRange, obRange.skip(1), obRange.skip(2), lambda s1,
               s2, s3: '%s / %s / %s' % (s1, s2, s3)).subscribe(ObserverClass())
```

zip은 복수의 Observable를 처리할 때에 사용되며, 같은 단계에서는 모든 Observable이 처리되어야 진행됩니다.

```python
obRange = Observable.range(0, 6)
Observable.zip_list(obRange, obRange.skip(
    1), obRange.skip(2)).subscribe(ObserverClass())
```

zip_list는 zip과 비슷하지만, 같은 단계를 리스트로 묶어줍니다.

```python
s1 = Observable.interval(0).map(lambda i: '%s' % i)
s2 = Observable.interval(0).map(lambda i: '%s' % i)
s1.combine_latest(s2, lambda s1, s2: '%s, %s'
                  % (s1, s2)).take(6).subscribe(ObserverClass())
```

combine_latest는 두 Observable의 마지막 이벤트를 같이 전달해줍니다.

```python
s1 = Observable.interval(0).map(lambda i: '%s' % i)
s2 = Observable.interval(0).map(lambda i: '%s' % i)
s1.with_latest_from(s2, lambda s1, s2: '%s, %s' %
                    (s1, s2)).take(6).subscribe(ObserverClass())
```

with_latest_from은 한 Observable에서 이벤트를 발행하면 전달해줍니다.

```python
s1 = Observable.interval(0).map(lambda i: '%s' % i)
s2 = Observable.interval(0).map(lambda i: '%s' % i)
s1.join(s2, lambda _: Observable.timer(10), lambda _: Observable.timer(
    0), lambda x, y: '%s %s' % (x, y)).take(6).subscribe(ObserverClass())
```

join은 정의된 시간동안 전달된 Observable들을 결합합니다.

```python
Observable.range(0, 3).select(lambda x: Observable.range(x, 3).map(
    lambda v: '%s (%s)' % (v, x))).switch_latest().subscribe(ObserverClass())
```

switch_latest는 전달받고 싶은 Observable로 스위칭할 수 있습니다.
