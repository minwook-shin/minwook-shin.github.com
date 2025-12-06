---
layout: post
title: RxPY Observable 생성하는 방법에 대하여 알아보기
---

오늘은 RxPY에서 Observable을 생성해주는 방법들을 실습해보려 합니다.

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
import random
import time
```

rx 패키지에서 전부 가져오고, random, time 패키지도 가져옵니다.

```python
class ObserverClass(Observer):

    def on_next(self, value):
        print("print : " + str(value))

    def on_completed(self):
        print("Done!")

    def on_error(self, error):
        print("Error : "+str(error))
```

Observer를 상속 받은 옵저버 클래스를 만들어줍니다. 여기에는 on_next, on_error, on_completed 메소드로 구성됩니다.

이를 이용해서 생성한 Observable들을 옵저버 클래스에 구독해줍니다.

```python
Observable.just({'answer': random.randint(0, 10)}).subscribe(ObserverClass())
```

just는 인자에 들어간 데이터들을 바로 Observable로 변환해줍니다.

```python
Observable.start(func=lambda: random.randint(0, 10)).subscribe(ObserverClass())
```

start는 함수의 결과를 바로 Observable로 변환해줍니다.

```python
Observable.range(0, 3).subscribe(ObserverClass())
```

range는 주어진 범위를 iterable한 Observable로 만들어줍니다.

```python
gen = (random.randint(0, 10) for j in range(3))
Observable.from_iterable(gen).subscribe(ObserverClass())
```

from_iterable은 엘리먼트들을 관찰가능한 Observable로 변환해줍니다.

```python
def g(f, a, b):
    f(a, b)
Observable.from_callback(lambda a, b, f: g(f, a, b))(
    'foo', 'bar').subscribe(ObserverClass())
```

from_callback도 from_iterable와 다를 것이 없습니다.

```python
Observable.repeat({'rand': time.time()}, 3).subscribe(ObserverClass())
```

repeat은 반복하여 Observable을 만듭니다.

```python
Observable.generate(0, lambda x: x < 5, lambda x:  x + 1.0,
                    lambda x: x * 2).subscribe(ObserverClass())
```

generate는 시작할 정수, 조건식, 초기식, 증감식을 사용하요 Observable을 만듭니다.

```python
Observable.defer(lambda: Observable.just(
    random.randint(0, 10))).subscribe(ObserverClass())
```

defer는 구독하는 시간에 Observable을 만듭니다.

```python
Observable.if_then(
    lambda: True, Observable.return_value(10), Observable.return_value(20)).subscribe(ObserverClass())
```

if_then은 첫번째 인자가 참일 경우와 거짓일 경우를 결과값으로 나눌 수 있습니다.

```python
Observable.empty().subscribe(ObserverClass())
```

empty는 출력되는 Observable이 존재하지 않습니다.

```python
Observable.never().subscribe(ObserverClass())
```

never는 empty와 같지만, 종료되는 시점도 않습니다.

```python
Observable.throw(ZeroDivisionError).subscribe(ObserverClass())
```

throw는 오류를 발생시킵니다.
