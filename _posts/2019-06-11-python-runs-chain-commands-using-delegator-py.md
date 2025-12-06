---
layout: post
title: Python 외부 명령어를 수행하는 Delegator.py 라이브러리 알아보기
---

오늘은 Python으로 새로운 프로세스를 만들어서 입출력을 연결하던 Subprocesses 패키지와 같이 외부 명령어를 수행하는 Delegator.py를 알아보려 합니다.

## Delegator.py 설치

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
pip install delegator.py
```

pip로 Delegator.py를 설치합니다.

## 기존 subprocess

기존의 파이썬 표준 라이브러리로 제공되었던 subprocess로 사용하는 예제는 아래와 같습니다.

```python
import subprocess
```

subprocess를 가져옵니다.

```python
lst = subprocess.run(["ls", "-l"])
print(lst.stdout)
```

run으로 수행해서 표준 출력을 반환할 수 있습니다.

## delegator 예제

```python
import delegator
```

delegator를 가져옵니다.

```python
lst2 = delegator.run('ls', "-l")
print(lst2.out)
```

run으로 ls 명령을 수행해서 출력을 반환합니다.

```python
c = delegator.run(['curl', 'www.google.com'], block=False)
```

만약 오래 걸리는 프로세스는 위와 같이 띄우고,

```python
c.block()
```

block()으로 블락시킬 수 있습니다.

```python
print(c.pid)
print(c.out)
print(c.return_code)
```

또한 해당 프로세스의 pid를 알 수 있으며, 출력과 반환 코드를 알 수 있습니다.

```python
try:
    c = delegator.chain('fortune | cowsay')
    print(c.out)
except FileNotFoundError:
    print("맥 : brew install cowsay / brew install fortune")
```

파이프라인을 위와 같이 구현할 수 있습니다.

```python
delegator.run('fortune').pipe('cowsay')
```

기존의 run 함수로도 할 수도 있습니다.

```python
c = delegator.chain('env | grep NEWENV', env={'NEWENV': 'FOO_BAR'})
print(c.out)
```

환경 변수를 등록할 수 있습니다.