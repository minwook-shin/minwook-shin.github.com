---
layout: post
title: python Flask-RESTPlus로 api 서버만들고, swagger 사용해보기
---

오늘은 python으로 작동되는 flask 웹 프레임워크 기반의 Flask-RESTPlus를 가져다가 json을 반환해주는 api 서버를 만들어보고, swagger도 적용해보려 합니다.

## Flask

flask는 파이썬 웹 프레임워크로서 웹앱을 만들 수 있습니다.

Werkzeug 툴킷과 Jinja2 템플릿 엔진으로 인해 웹 애플리케이션을 만들 수 있게 됩니다.

```
pip3 install Flask
```

위와 같이 설치가 가능하며, 이 포스팅에서는 flask-restplus를 설치할 것이기 때문에 Flask는 자동으로 의존성이 됩니다.

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello World!"

app.run()
```

flask는 기본적으로 위와 같이 작성됩니다.

라우터를 할당해주면 해당 주소에 접근할 때마다 지정된 함수가 수행됩니다.

## Flask-RESTPlus

REST API를 빠르게 빌드하고, 여러 기능들이 기본 탑제되있는 Flask 확장 프레임워크이므로 Flask에 익숙하다면 쉽게 접근할 수 있습니다.

기본적으로 Swagger가 같이 포함되어 있어서 API 문서나 도구 모음을 제공합니다.

```
pip3 install flask-restplus
```

pip3설치하면 되며, flask와 같은 기반 프레임워크들은 의존성으로 같이 설치되게 됩니다.

## swagger

기존에 api 문서를 작업하고 포스트맨으로 검사하던 것들을 서버에 Swagger를 올려서 코드에 의해 같이 관리됩니다.

## code

```python
from flask import Flask
from flask_restplus import Resource, Api
```

Flask 앱을 초기화하기 위해 flask 패키지를 가져오고, 오늘의 메인인 flask_restplus 패키지에서 Resource, Api만 가져옵니다.

```python
app = Flask(__name__)
api = Api(app, version='1.0', title='API title',
          description='A simple API',
          )
```

app을 초기하고, 앱으 버전, 타이틀, 설명을 적습니다.

이는 웹서버를 구동했을 때에 최상위도 출력됩니다.

```python
ns = api.namespace('custom', description='operations')
```

네임 스페이스의 이름과 설명을 지정하여 라우터를 따로 묶어서 관리할 수 있습니다.

```python
app.config.SWAGGER_UI_DOC_EXPANSION = 'full'
```

페이지에 출력될 SWAGGER UI의 문서가 기본적으로 접혀있지만, 위와 같이 full로 설정하면 서버를 구동하자마자 모든 정보가 열려보입니다.

네임 스페이스가 닫힌 상태는 기본 상태인 "none"이며, 라우터 리스트만 보이는 상태를 "list"라고 합니다.

```python
@api.route('/<string:text>')
@api.response(200, 'Found')
@api.response(404, 'Not found')
@api.response(500, 'Internal Error')
@api.param('text', 'String value')
```

라우터에는 위와 같이 작성하게 되면 해당 라우터에 대한 정보를 추가할 수 있습니다.

위에서 지정한 api에서 호출하면 기본적인 네임 스페이스에 소속되며, 경로, 응답 코드, 인자등을 표기할 수 있습니다.

이는 실제 동작은 물론이고, SWAGGER에도 같이 출력되어 동시에 문서화도 진행됩니다.

```python
class Algorithm(Resource):
    @api.doc('get')
    def get(self, text):
        return {"text": "hello,world"}

    @api.doc('delete')
    def delete(self, id):
        return '', 204

    @api.doc('put')
    def put(self, id):
        return ""
```

Resource를 상속받은 클래스로 get, post, delete를 구현할 수 있습니다.

함수의 반환 값에 의해 기능이 동작하게 됩니다.

```python
@ns.route('/<string:text>')
@ns.response(200, 'Found')
@ns.response(404, 'Not found')
@ns.response(500, 'Internal Error')
@ns.param('text', 'String value')
class Algorithm(Resource):
    @ns.doc('get')
    def get(self, text):
        return {"text": "hello,world"}

    @ns.doc('delete')
    def delete(self, id):
        return '', 204

    @ns.doc('put')
    def put(self, id):
        return ""
```

만약 네임 스페이스를 만들었다면 위와 같이 기본적인 api말고, 만든 변수를 사용하면 됩니다.

```python
if __name__ == '__main__':
    app.run(host='127.0.0.1', port=5000, debug=True)
```

직접 해당 파일을 실행했을 때는 메인 함수에 의해서 로컬호스트인 127.0.0.1의 5000 포트로 디버깅 서버가 구동됩니다.

```
http://127.0.0.1:5000/custom/a
```

네임 스페이스를 지정한 get함수에 접근하기 위해 위 주소를 get해줍니다.

```json
{
  "text": "hello,world"
}
```

반환 값이 같기 때문에 위 json이 출력되게 됩니다.
