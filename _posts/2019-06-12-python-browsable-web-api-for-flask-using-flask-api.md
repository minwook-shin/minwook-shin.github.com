---
layout: post
title: Python 브라우저 api 구현하는 Flask API 라이브러리 알아보기
---

오늘은 Python으로 Django REST 프레임워크와 같이 비슷한 브라우저 API의 구현을 제공하는 Flask API를 알아보려 합니다.

## Flask API 설치

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
pip install Flask-API
```

pip로 Flask API를 설치합니다.

## api 만들기

```python
from flask import request, url_for
from flask_api import FlaskAPI, status
```

flask와 flask_api에서 필요한 것들을 가져옵니다.

```python
app = FlaskAPI(__name__)
```

FlaskAPI 객체를 만듭니다.

```python
url_content = {
    0: 'main',
    1: 'shop',
    2: 'profile',
}
```

기본적으로 url에 들어갈 text 값들을 정의해둡니다.

```python
def content_repr(key):
    return {
        'url': request.host_url.rstrip('/') + url_for('url_detail', key=key),
        'text': url_content[key]
    }
```

반환할 컨텐츠의 딕셔너리를 값들을 랩핑할 함수를 만듭니다.

미리 만들어둔 url에 들어갈 text 값들을 키로 불러올 수 있게 되었습니다.

```python
@app.route("/", methods=['GET'])
def url_list():
    return [content_repr(idx) for idx in sorted(url_content.keys())]
```

GET할 수 있는 함수를 만듭니다.

url_content의 키들을 가져와서 content_repr에 넣어주면 순서대로 딕셔너리가 반환되어 리스트로 반환됩니다.

해당 라우터로 접근하면 Url list으로 나타나고, 아래와 같이 출력됩니다.

```bash
GET http://127.0.0.1:5000/
```

```json
HTTP 200 OK
Content-Type: application/json

[
    {
        "url": "http://127.0.0.1:5000/0/",
        "text": "main"
    },
    {
        "url": "http://127.0.0.1:5000/1/",
        "text": "shop"
    },
    {
        "url": "http://127.0.0.1:5000/2/",
        "text": "profile"
    }
]
```

이제 GET 뿐만 아니라 'PUT', 'DELETE'도 할 수 있는 라우터를 만들어보려 합니다.

```python
@app.route("/<int:key>/", methods=['GET', 'PUT', 'DELETE'])
def url_detail(key):
    if request.method == 'PUT':
        note = str(request.data.get('text', ''))
        url_content[key] = note
        return content_repr(key)
```

'GET', 'PUT' 그리고 'DELETE'까지 할 수 있는 함수를 만듭니다.

만약 PUT을 하면 url_content의 내용이 교체됩니다.


```python
    elif request.method == 'DELETE':
        url_content.pop(key, None)
        return '', status.HTTP_204_NO_CONTENT
```

DELETE를 하면 url_content에서 특정 키 값을 빼냅니다.

```python
    return content_repr(key)
```

기본적으로 특정 키의 값을 반환합니다.

해당 라우터로 접근하면 Url detail이라고 나타나고, 아래와 같이 출력됩니다.

```bash
GET http://127.0.0.1:5000/0/
```

```json
HTTP 200 OK
Content-Type: application/json

{
    "url": "http://127.0.0.1:5000/0/",
    "text": "main"
}
```

마지막으로 메인함수에서 앱을 수행하면 됩니다.

```python
if __name__ == "__main__":
    app.run()
```

로컬 개발 서버를 구동하게 됩니다.