---
layout: post
title: RxPY Observable 변환하는 방법에 대하여 알아보기
---

오늘은 RxPY에서 Observable을 변환해주는 방법들을 실습해보려 합니다.

RxPY는 Reactive Extensions의 python 라이브러리입니다.

## RxPY 설치

```
pip3 install rx
```

rx-1.6.x 버전이 설치되며,

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
Observable.from_((1, 2, 3)).map(lambda x: x * 2).subscribe(ObserverClass())
```

```python
Observable.from_(range(10, 20, 2)).map(
    lambda x, i: "%s / %s" % (i, x * 2)).subscribe(ObserverClass())
```

가장 기본적으로 변환하려면 map이 있습니다.

람다식을 지나서 Observable이 원하는 형태로 변환됩니다.

```python
Observable.range(1, 2).flat_map(
    lambda x: Observable.range(x, 2)).subscribe(ObserverClass())
```

flat_map은 람다식을 지나서 Observable이 원하는 형태로 변한 후에 다시 Observable로 변환됩니다.

```python
Observable.range(1, 2).flat_map_latest(
    lambda x: Observable.range(x, 2)).subscribe(ObserverClass())
```

flat_map_latest는 새로운 Observable이 생겼을 때에 기존의 Observable은 무시됩니다.

```python
def sel(*arr):
    return '-'.join([str(i) for i in arr])

Observable.from_(('a', 'b', 'c')).flat_map(lambda x, i: (
    x, i), sel).subscribe(ObserverClass())
```

flat_map에 selector를 사용할 수도 있습니다.

```python
Observable.from_([{'x': 1, 'y': 2}, {'x': 3, 'y': 4}]
                 ).pluck('y').subscribe(ObserverClass())
```

기존의 Observable에서 pluck로 원하는 것만 뽑아서 Observable을 변환할 수 있습니다.

```python
class test:
    def __init__(self, x, y):
        self.x = x
        self.y = y

Observable.from_([test(1, 2), test(3, 4)]).pluck_attr(
    'x').subscribe(ObserverClass())
```

pluck_attr으로 속성 값을 추출해서 Observable을 변환할 수 있습니다.

```python
Observable.from_(('a', 'b', 'c')).to_blocking(
).many_select(lambda x: x.first()).merge_all().subscribe(ObserverClass())
```

many_select는 원본 Observable에서 방출된 각 인덱스로 인하여 순서대로 Observable을 변환할 수 있습니다.

```python
Observable.from_(('1', '2', '3')).to_blocking().scan(lambda x,
                                                     y: int(x) + int(y)).subscribe(ObserverClass())
```

scan으로 람다식에 의해 Observable의 기존의 x에 값을 유지하고 y를 더해줍니다.

```python
Observable.from_(('1', '2', '3')).to_blocking(
).timestamp().pluck_attr('timestamp').subscribe(ObserverClass())
```

timestamp로 변환된 Observable에서 timestamp만 뽑아서 구독합니다.

```python
Observable.from_(('1', '2', '3')).to_blocking().time_interval().map(
    lambda x: x.interval).subscribe(ObserverClass())
```

time_interval로 연속적으로 Observable의 변환이 일어나는 시간 간격을 구독합니다.
