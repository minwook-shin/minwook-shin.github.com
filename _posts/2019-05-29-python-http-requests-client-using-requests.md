---
layout: post
title: Python HTTP 요청 requests 라이브러리 알아보기
---

오늘은 Python으로 http 요청하는 라이브러리인 requests를 적용해보려 합니다.

## requests 설치

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
 pip install requests
```

pip로 requests를 설치합니다.

## get

```python
import requests


r = requests.get('https://api.github.com')
print(r)
print(r.content, type(r.content))
print(r.text, type(r.text))
print(r.url)
print(r.headers)
print(r.status_code)
```

특정 url을 get으로 가져올 수 있습니다.

또한 content는 바이트, text는 문자열로 가져옵니다.

url은 최종으로 가져올 때의 주소, headers는 응답 헤더를 반환합니다.

## post

```python
import requests


r = requests.post('https://httpbin.org/post', data={'key': 'value'})
print(r)
print(r.content, type(r.content))
print(r.text, type(r.text))
print(r.url)
print(r.headers)
print(r.status_code)
```

특정 url을 post로 가져올 수 있습니다.

또한 content는 바이트, text는 문자열로 가져옵니다.

url은 최종으로 가져올 때의 주소, headers는 응답 헤더를 반환합니다.

## put

```python
import requests


r = requests.put('https://httpbin.org/put', data={'key': 'value'})
print(r)
print(r.content, type(r.content))
print(r.text, type(r.text))
print(r.url)
print(r.headers)
print(r.status_code)
```

```python
import requests


payload = {'key1': 'value1', 'key2': 'value2'}
r = requests.get('https://httpbin.org/get', params=payload)
print(r)
print(r.content, type(r.content))
print(r.text, type(r.text))
print(r.url)
print(r.headers)
print(r.status_code)
```

특정 url을 get으로 가져오면서 파라미터를 같이 전달할 수 있습니다.

또한 content는 바이트, text는 문자열로 가져옵니다.

url은 최종으로 가져올 때의 주소, headers는 응답 헤더를 반환합니다.

## 헤더

```python
import requests


headers = {'user-agent': "Mozilla/5.0"}
r = requests.get('https://www.google.com/search?q=python', headers=headers)
```

헤더를 같이 첨부해서 http 요청을 남길 수 있습니다.

## 쿠키

```python
import requests


r = requests.get('https://httpbin.org/cookies', cookies=dict(hello_cookies='hello,world!'))
print(r.text)
```

쿠키를 첨부할 수 있습니다.
