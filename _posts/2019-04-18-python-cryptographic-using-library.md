---
layout: post
title: Python cryptographic 라이브러리 알아보기
---

오늘은 Python으로 문자열을 암호화할 수 있는 cryptographic 패키지에 대하여 알아보려 합니다.

## 개요

암호화 알고리즘이 구현된 패키지입니다.

## cryptography 설치

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
pip install cryptography
```

pip로 cryptography를 설치합니다.

## hello, world

```python
from cryptography.fernet import Fernet
```

암호화와 복호화에 같은 키 값을 쓰는 대칭키 암호화 인증 방식인 Fernet을 가져옵니다.

```python
key = Fernet.generate_key()
```

키를 생성합니다.

```python
token = Fernet(key).encrypt(b"hello, world")
print(token)
```

hello, world라는 바이트를 키로 암호화하고, 복호화 토큰을 생성합니다.

```python
decrypt_string = Fernet(key).decrypt(token)
print(decrypt_string)
```

해당 토큰과 키만 있다면 다시 복호화할 수 있습니다.

## AES-GCM

```python
import os
from cryptography.hazmat.primitives.ciphers.aead import AESGCM
```

무결성과 기밀성이 존재하는 Galois Counter Mode의 AES인 AES-GCM를 가져옵니다.

```python
data = b"a secret message"
```

바이트를 정의합니다.

```python
key = AESGCM.generate_key(bit_length=128)
```

블록 크키가 128에 대한 키에 대해 생성합니다.

```python
os_urandom = os.urandom(12)
```

임의의 바이트를 생성합니다.

```python
encrypt_string = AESGCM(key).encrypt(os_urandom, data, b"authenticated")
print(encrypt_string)
```

생성한 키를 가지고 임의의 바이트와 데이터, 그리고 AEAD(인증 값)를 함께 암호화 합니다.

```python
decrypt_string = AESGCM(key).decrypt(os_urandom, encrypt_string, b"authenticated")
print(decrypt_string)
```

생성한 키를 가지고 임의의 바이트와 데이터, 그리고 AEAD(인증 값)를 함께 복호화 합니다.

## Ed25519

```python
import cryptography.exceptions
from cryptography.hazmat.primitives.asymmetric.ed25519 import Ed25519PrivateKey
```

Ed25519 개인 키를 만들 수 있는 Ed25519PrivateKey를 가져옵니다.

그리고 개인 키가 일치하지 않았을 때에 오류를 막을 수 있는 cryptography.exceptions도 가져옵니다.

```python
private_key = Ed25519PrivateKey.generate()
```

개인 키를 생성합니다.

```python
signature = private_key.sign(b"example@example.com")
```

생성된 개인 키에 서명합니다.

```python
public_key = private_key.public_key()
```

공개 키를 생성합니다.

```python
try:
    verify_result = public_key.verify(signature, b"example@example.com")
except cryptography.exceptions.InvalidSignature:
    print("Wrong key")
```

try except로 같은 서명이 아닐 시에는 InvalidSignature 오류가 발생합니다.
