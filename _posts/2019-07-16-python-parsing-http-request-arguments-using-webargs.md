---
layout: post
title: Python HTTP 요청 쿼리 파싱하는 webargs 라이브러리 알아보기
---

오늘은 Python에서 HTTP 요청 쿼리 파싱하고 검증할 수 있는 webargs 라이브러리를 알아보려 합니다.

## webargs 설치

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
pip install -U Flask
```

해당 라이브러리는 Flask 서버에서 적용할 수 있으므로 Flask 먼저 설치합니다.

```
pip install -U webargs
```

pip로 webargs를 설치합니다.

## hello world

```python
from flask import Flask
from webargs import fields
from webargs.flaskparser import use_args
```

flask와 webargs를 가져옵니다.

```python
app = Flask(__name__)
```

Flask 앱을 초기화합니다.

```python
hello_field = {"hello": fields.Str(required=True)}
```

문자열로 받을 URL 쿼리로 받을 데이터 필드를 만들 수 있습니다.

필수로 받을 값은 required로 참을 설정해야 합니다.

```python
@app.route("/")
@use_args(hello_field)
def index(args):
    return "Hello " + args["hello"]
```

테코레이터로 루트 경로에 접근할 때 특정 쿼리 파라미터를 설정할 수 있습니다.

```python
if __name__ == "__main__":
    app.run()
```

메인 함수를 실행하면 서버를 구동할 수 있게 합니다.

서버가 켜지면 curl 명령어로 http://localhost:5000/\?name\='World' 쿼리 파라미터를 넘겨줄 수 있습니다.

## login example

로그인할 때에 사용자 이름과 비밀번호를 건내주면 json으로 반환하는 예제를 작성해보려 합니다.

```python
from flask import request, Flask, jsonify
from webargs import fields
from webargs.flaskparser import parser
```

flask와 webargs를 가져옵니다.

```python
app = Flask(__name__)
```

Flask 앱을 초기화합니다.

```python
login_field = {
    "username": fields.Str(required=True),
    "password": fields.Str(required=True, validate=lambda p: len(p) >= 5),
}
```

사용자 이름과 비밀번호를 문자열로 받는 데이터 필드를 만들고, 비밀번호는 5자리 이상인지를 검증할 수 있게 작성했습니다.

```python
@app.route("/login", methods=["POST"])
def register():
    args = parser.parse(login_field, request)
    return jsonify({"id": args["username"], "password": len(args["password"]) * "*"})
```

POST로 login을 수행하면서 값들을 보낼 수도 있습니다.

```python
if __name__ == "__main__":
    app.run()
```

메인 함수를 실행하면 서버를 구동할 수 있게 합니다.

서버가 켜지면 curl 명령어로 "username=test_id&password=12345" 값을 -d   옵션으로 보내고, http://localhost:5000/login 을 수행할 수 있습니다.