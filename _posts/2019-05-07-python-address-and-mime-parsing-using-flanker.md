---
layout: post
title: Python 이메일 주소와 mime 파싱하는 Flanker 라이브러리 알아보기
---

오늘은 Python에서 이메일 주소와 전송 표준 포맷 mime 형식을 파싱할 수 있는 라이브러리인 Flanker 패키지에 대하여 간단히 알아보려 합니다.

## Flanker 설치

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
pip install flanker
```

pip로 Flanker를 설치합니다.

## 이메일 주소 파싱

```python
from flanker.addresslib import address
```

flanker의 address를 가져옵니다.

```python
ap = address.parse('Example example@example.com')
print(ap)

not_email = address.parse('Example @example.com')
print(not_email)
```

메일에 표시되는 이름과 이메일 주소를 문자열로 넣으면 이메일인 순간만 출력됩니다.

```python
multi_address = address.parse_list('example1@example.com, example2@example.com')
print(multi_address)

multi_address2 = address.parse_list('example1@example.com, example2@example.com', as_tuple=True)
print(multi_address2)

multi_address3 = address.parse_list('example1@example.com, example2@example.com', strict=True)
print(multi_address3)
```

여러 이메일 리스트도 가능합니다.

튜플로 변환할 수 있습니다.

## 이메일 주소 검사기

```python
from flanker.addresslib import validate
```

flanker의 validate를 가져옵니다.

```python
sa = validate.suggest_alternate('example@gmail..com')
print(sa)
```

올바르지 못한 이메일이면 대체 이메일을 제시해줄 수 있습니다.

## mime

```python
from flanker import mime
```

flanker의 mime를 가져옵니다.

```python
msg = '''MIME-Version: 1.0
Content-Type: text/plain
From: Example1 <example1@example.com>
To: Example2 <example2@example.com>
Subject: hello, message
Date: Mon, 6 May 2019 12:43:03 -0700

this is message.'''
```

mime 표준 포맷으로 저장한 내용을 문자열로 저장합니다.

```python
fs = mime.from_string(msg)
```

문자열로 되어 있는 것을 mime 형식으로 변환할 수 있습니다.

```python
print(fs.body)
print(fs.headers.items())
print(fs.content_type)
print(fs.subject)
```

본문 내용과 헤더를 나눌 수 있으며, 주제와 content_type도 나눌 수 있습니다.

```python
from flanker.mime import create
```

flanker의 create를 가져옵니다.

```python
message = create.text("plain", "hello, world!")
message.headers['From'] = u'Example1 <example1@example.com>'
message.headers['To'] = u'Example2 <example2@example.com>'
message.headers['Subject'] = u"hello"

print(message.to_string())
```

이번에는 반대로 메시지를 조합하여 mime 형식으로 만들어보려 합니다.

본문과 헤더를 만들고 to_string으로 출력할 수 있습니다.
