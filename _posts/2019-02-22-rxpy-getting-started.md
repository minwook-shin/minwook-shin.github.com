---
layout: post
title: RxPY 실습하면서 알아보기
---

오늘은 비동기 프로그래밍을 구현할 수 있는 Reactive Extensions을
python에서 적용할 수 있는 RxPY를 설치하고 실습해보려 합니다.

RxPY는 비동기 프로그래밍을 구현할 수 있는 Reactive Extensions의 python 라이브러리입니다.

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

## import

```python
from rx import Observable, Observer
```

rx 패키지를 가져와서 Observable과 Observer를 사용합니다.

```python
from rx.subjects import Subject
```

Subject를 만들 수 있게 패키지를 추가합니다.

## Generating a sequence

```python
class ObserverClass(Observer):
    def on_next(self, value):
        print(value)

    def on_error(self):
        pass

    def on_completed(self):
        pass
```

Observer를 상속 받은 옵저버 클래스를 만들어줍니다. 여기에는 on_next, on_error, on_completed 메소드로 구성됩니다.

```python
src = Observable.from_iterable(range(5))
src.subscribe(ObserverClass())
```

from_iterable 메소드를 사용하면 range의 엘리먼트들이 관찰가능한 Observable로 변환해줍니다.

```python
src = Observable.from_iterable(range(5))
src.subscribe(print)
```

직접 구성한 옵저버 클래스말고 print를 이용하여 출력해도 됩니다.

```python
src = Observable.from_iterable(range(5))
src.subscribe(lambda x: print(x))
```

위 print를 lambda로 풀어쓰면 위와 같이 코드를 작성해도 됩니다.

## Filtering a sequence

```python
def func(x):
    return x % 2
```

함수를 만들어줍니다.

```python
src = Observable.from_(range(5))
src.filter(lambda x: func(x)).subscribe(print)
```

만들어진 함수를 대입하여 filter로 시퀀스를 필터링하는 과정을 구독했습니다.

## Transforming a sequence

```python
def func(x, i):
    print(i, ":", end=' ')
    return x * 2
```

필터링했을 때처럼 함수를 만들어줍니다.
다만, 이번에는 변환이므로 함수를 약간 보기 좋게 수정해줍니다.

```python
src = Observable.from_(range(5))
src.map(func).subscribe(print)
```

만들어진 함수를 대입하여 map으로 시퀀스를 변환할 수 있는 과정을 구독했습니다.

## Merge

```python
src1 = Observable.range(1, 5)
src2 = Observable.from_("hello")
src1.merge(src2).subscribe(print)
```

각각 range와 from\_으로 두개의 Observable을 만들어서 병합합니다.

병합되어서 구독했을 때에는 각 Observable의 순서만 지켜진 채로 진행됩니다.

## Subjects and Streams

```python
ob = Subject()
ob.on_next(1)
sub1 = ob.subscribe(print)
ob.on_next(2)
sub2 = ob.subscribe(print)
sub1.dispose()
ob.on_next(3)
sub2.dispose()
```

Subject는 Observable의 일종으로 여러개의 옵저버가 구독할 수 있습니다.

on_next로 다음에 전파될 데이터를 넘겨받습니다.

첫 옵저버는 2 다음에 바로 dispose 하고, 다음 옵저버는 3 다음에 바로 dispose 하므로 2와 3이 출력됩니다.
