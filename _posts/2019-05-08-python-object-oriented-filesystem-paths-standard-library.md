---
layout: post
title: Python 객체 파일 경로 pathlib 라이브러리 알아보기
---

오늘은 Python에서 파일 경로를 객체로 저장하고 사용하는 표준 라이브러리인 pathlib 패키지에 대하여 간단히 알아보려 합니다.

파이썬 3.4 버전부터 지원합니다.

## pathlib 설치

파이썬 3.4부터 기본적으로 탑제되어 있습니다.

## 기본 예제

```python
from pathlib import Path
```

pathlib의 Path를 가져옵니다.

```python
p = Path('.')
for x in p.iterdir():
    print(x)
```

현재 경로를 기점으로 Path 객체를 만들어서 폴더에 있는 파일과 폴더들을 iterator로 반환합니다.

반복문에서 사용할 수 있습니다.

```python
p = Path('.')
for x in p.iterdir():
    if x.is_dir():
        print(x)
```

is_dir로 폴더만 골라낼 수 있습니다.

```python
p = Path('.')
for x in p.iterdir():
    if not x.is_dir():
        print(x)
```

반대로 파일만 골라낼 수 있습니다.

```python
p = Path('.')
pyList = list(p.glob('*.py'))
print(pyList)
```

특정 조건에 맞는 파일의 목록을 생성할 때에 사용합니다.

```python
p = Path('/etc')
exist = p.exists()
isDir = p.is_dir()
print(exist)
print(isDir)
```

특정 폴더가 존재하는지와 폴더인지 확인할 수 있습니다.

```python
p = Path('./python-rx-basic.py')
with p.open() as f:
    print(f.readline())
```

특정 파일의 내용을 열어서 줄 단위로 읽을 수 있습니다.

```python
cwd = Path.cwd()
print(cwd)
```

현재 경로를 확인할 수 있습니다.

```python
home = Path.home()
print(home)
```

홈폴더를 확인할 수 있습니다.

```python
from pathlib import PurePath

pure = PurePath('setup.py')
print(pure)
```

실제 파일의 경로에 접근하지 않는 PurePath 객체를 사용할 수 있습니다.

```python
from pathlib import PurePath

ex1 = PurePath('a', 'b/c', 'd')
print(ex1)
ex2 = PurePosixPath('a/b/c/d')
print(ex2)
ex3 = PurePath(Path('a'), Path('b'), Path('c'), Path('d'))
print(ex3)
print(ex1 == ex2 == ex3)
```

파일의 경로도 비교할 수 있습니다.

## PurePath 예제

```python
from pathlib import PureWindowsPath, PurePosixPath, WindowsPath
```

pathlib의 PureWindowsPath, PurePosixPath 그리고 WindowsPath를 가져옵니다.

```python
print(PurePosixPath('/etc'))
print(PureWindowsPath('c:/Windows'))
```

플랫폼마다 다른 경로를 PurePath로 나타낼 수 있습니다.

```python
try:
    print(WindowsPath('setup.py'))
except NotImplementedError as e:
    print("cannot instantiate 'WindowsPath' on your system")
```

WindowsPath를 리눅스나 맥에서 수행하면 NotImplementedError가 출력됩니다.

PureWindowsPath를 사용해야 합니다.
