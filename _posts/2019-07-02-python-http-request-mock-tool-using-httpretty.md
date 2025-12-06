---
layout: post
title: Python http 요청 mocking하는 HTTPretty 라이브러리 알아보기
---

오늘은 Python에서 가상의 http 요청을 만들어서 테스트할 수 있는 HTTPretty 라이브러리를 알아보려 합니다.

## HTTPretty 설치

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
pip install httpretty
```

pip로 HTTPretty를 설치합니다.

## 예제

```python
import httpretty
import urllib.request
import json
```

http 를 mock으로 만들어서 테스트할 수 있는 httpretty 패키지와 페이지 내용을 가지오는 urllib, 그리고 json 형식으로 로드하기 위한 json 패키지를 가져옵니다.

```python
@httpretty.activate
def test_http():
    httpretty.register_uri(
        httpretty.GET,
        "https://example.com",
        body='{"value": "hello,world"}'
    )
```

httpretty로 테스트할 함수를 만들 수 있습니다.

register_uri로 get 형식의 통합 자원 식별자를 등록합니다.

위 코드는 https://example.com 으로 접속했을 때에 hello,world라고 출력되도록 mock을 작성했습니다.

```python
    with urllib.request.urlopen('https://example.com') as f:
        response = f.read()
```

urllib.request로 urlopen을 사용하여 https://example.com 의 내용을 가져올 수 있습니다.

```python
    content = json.loads(response.decode())
    print(json.loads(response.decode()))

    print(content == {'value': 'hello,world'})
```

json.loads()로 문자열을 파이썬 dict 타입으로 변환합니다.

```python
if __name__ == '__main__':
    test_http()
```

이제 메인 함수를 작성하여 httpretty로 감싼 함수를 수행합니다.