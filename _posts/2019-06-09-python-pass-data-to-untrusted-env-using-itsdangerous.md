---
layout: post
title: Python 문자열 서명하거나 암호화하는 itsdangerous 라이브러리 알아보기
---

오늘은 Python으로 문자열이나 변수들을 서명하거나 암호화하거는 itsdangerous를 알아보려 합니다.

## itsdangerous 설치

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
pip install -U itsdangerous
```

pip로 itsdangerous를 설치합니다.

## hello world

```python
from itsdangerous import URLSafeSerializer
```

URLSafeSerializer를 가져옵니다.

```python
auth_s = URLSafeSerializer("secret key", "auth")
token = auth_s.dumps({"id": 1, "name": "name"})
print(token)
```

URLSafeSerializer로 비밀키와 salt 값을 넣습니다.

문자열이 아닌 변수를 암호화해서 토큰을 생성할 수 있습니다.

```python
data = auth_s.loads(token)
print(data["name"])
```

암호 키를 가지고 다시 변수를 가져올 수 있습니다.

## sign

```python
from itsdangerous import Signer, BadSignature
```

Signer를 가져오고, 추가로 오류를 잡기 위한 BadSignature도 가져옵니다.

```python
s = Signer("secret-key")
signed_text = s.sign("test string")
print(s.unsign(signed_text))
try:
    s.unsign(b"different string.wh6tMHxLgJqB6oY1uT73iMlyrOA")
except BadSignature:
    print("Signature does not match")
```

Signer 객체로 문자열을 sign하면 신뢰도를 가지게 할 수 있습니다.

unsign하여 원래 문자열을 볼 수 있습니다.

만약 사인이 다르면 BadSignature라고 오류를 발생합니다.

```python
from itsdangerous import TimestampSigner, SignatureExpired
import time
```

일정 시간만 유효한 TimestampSigner를 가져오고, 사인의 기간이 만료되었을 때에 발생하는 SignatureExpired 오류도 가져옵니다.

```python
s = TimestampSigner('secret-key')
signed_text = s.sign('text')
time.sleep(2)
try:
    print(s.unsign(signed_text, max_age=1))
except SignatureExpired:
    print("SignatureExpired")
```

time 패키지로 지연하고 다시 unsign하면 SignatureExpired 오류가 발생합니다.

## serializer

```python
from itsdangerous.serializer import Serializer
s = Serializer("secret-key")
serializer_value = s.dumps([1, 2, 3, 4])
print(s.loads(serializer_value))
```

Serializer로 문자열이 아닌 것들도 서명할 수 있습니다.

```python
from itsdangerous.url_safe import URLSafeSerializer

s1 = URLSafeSerializer("secret-key", salt="activate")
s1.dumps(1)
s2 = URLSafeSerializer("secret-key", salt="upgrade")
s2.dumps(1)
```

URLSafeSerializer는 암호 키와 salt 값로 암호화할 수 있습니다.

추가로 salt 값이란 암호화를 위한 추가 문자열이라고 합니다.

```python
print(s1.loads(s1.dumps(1)))
print(s2.loads(s2.dumps(1)))
```

loads로 다시 원본을 반환할 수 있습니다.