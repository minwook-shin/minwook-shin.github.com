---
layout: post
title: Python ssh 프로토콜 paramiko 라이브러리 알아보기
---

오늘은 Python으로 ssh2 프로토콜이 구현된 paramiko 패키지에 대하여 알아보려 합니다.

## paramiko 설치

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

```
pip install paramiko
```

pip로 paramiko를 설치합니다.

## ssh 접속

```python
from paramiko.py3compat import input
import paramiko
import getpass
import sys
```

패키지들은 가져옵니다.

```python
paramiko.util.log_to_file("logging.log")
```

로그를 남길 파일을 지정합니다.

```python
hostname = input("Host: ")
```

호스트 이름을 입력 받습니다.

```python
if len(hostname) == 0:
    sys.exit(1)
```

만약 호스트 이름으로 입력된 값이 없다면 종료합니다.

```python
if hostname.find(":") >= 0:
    hostname, port_str = hostname.split(":")
    port = int(port_str)
```

호스트 이름을 받을 때에 포트 이름도 같이 넣어져 있다면 분리합니다.

```python
default_username = getpass.getuser()
```

환경의 유저 이름을 가져옵니다.

```python
username = input("Username: ")
if len(username) == 0:
    username = default_username
```

유저 이름을 입력받고 못 받는다면, getpass에 의해 받은 기본 유저 이름을 사용합니다.

```python
password = getpass.getpass("Password: ")
```

비밀번호를 입력합니다.

```python
client = paramiko.SSHClient()
```

ssh 클라이언트 객체를 생성합니다.

```python
client.load_system_host_keys()
```

시스템에서 호스트 키를 불러옵니다.

```python
client.set_missing_host_key_policy(paramiko.WarningPolicy())
```

호스트 키가 없을 때에 사용할 정책을 정합니다.

```python
client.connect(hostname, port, username, password)
```

적은 호스트 이름과 포트 그리고 유저 이름과 마지막으로 비밀번호를 가지고 ssh 접속합니다.

```python
client.close()
```

모든 작업이 완료되면 클라이언트 객체를 소멸합니다.
