---
layout: post
title: Python Sklearn의 joblib 라이브러리 알아보기
---

오늘은 Python으로 디스크 캐싱, 병렬 프로그래밍하거나, 학습한 모델을 저장할 수 있는 Sklearn의 joblib를 알아보려 합니다.

## joblib 설치

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
pip install joblib
```

pip로 joblib를 설치합니다.

## memory

```python
from joblib import Memory
```

캐싱하기 위해서 Memory를 가져옵니다.

```python
cache_dir = './cache_dir'
memory = Memory(cache_dir, verbose=0)
```

캐싱을 할 폴더를 지정하고 Memory객체를 생성합니다.

```python
@memory.cache
def func(x):
    print('Running (%s)' % x)
    return x
```

캐싱할 func 함수를 만듭니다.

```python
print(func(1))
print(func(1))
print(func(2))
```

같은 값이 연이어 들어가면 캐싱된 값이 반환되므로 출력 문구는 생략됩니다.

```json
{ "duration": 0.0007112026214599609, "input_args": { "x": "2" } }
```

캐싱을 한 폴더에 접근해보면 메타데이터로 남겨져 있는 것을 볼 수 있습니다.

## Parallel

```python
from math import sqrt

for i in range(10):
    print(sqrt(i ** 2))
```

위 코드를 병렬처리할 수 있습니다.

```python
import time
from math import sqrt
from joblib import Parallel, delayed
```

우선 병렬 처리를 위해서 Parallel를 가져옵니다.

```python
lst = Parallel(n_jobs=2,prefer="threads")(delayed(sqrt)(i ** 2) for i in range(10))
print(lst)
```

기본적으로 joblib.Parallel은 loky 백엔드 모듈을 사용하지만,
preferl = "threads"를 인자로 전달하는 경우에는 쓰레드를 사용하게 됩니다.

## dump and load

```python
import joblib
```

파일로 저장하고 다시 읽기 위해 joblib를 가져옵니다.

```python
filename = './test.joblib'
```

쓸 파일의 이름을 정합니다.

```python
lst = [('text', [1, 2, 3])]
```

넣어질 리스트를 정의합니다.

```python
joblib.dump(lst, filename)

print(joblib.load(filename))
```

dump로 저장하고, load로 불러와서 반환합니다.

```python
joblib.dump(lst, filename + '.gz', compress=('gzip', 3))

print(joblib.load(filename + '.gz'))
```

기본적으로 joblib.dump()는 zlib 압축 방법을 사용하지만, gzip으로도 가능합니다.

```python
import gzip
with gzip.GzipFile(filename + '.gz', 'wb', compresslevel=3) as f:
    joblib.dump(lst, fo)
with gzip.GzipFile(filename + '.gz', 'rb') as f:
    print(joblib.load(fo))
```

파이썬의 기본 라이브러리인 gzip으로도 압축할 수 있습니다.
