---
layout: post
title: Python REST api 구현하는 Flask-RESTful 라이브러리 알아보기
---

오늘은 Python으로 GET, POST, PUT, DELETE가 가능한 REST API를 구현하기 해주는 Flask-RESTful를 알아보려 합니다.

## Flask-RESTful 설치

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
pip install flask-restful
```

pip로 Flask-RESTful을 설치합니다.

## hello world

루트 라우터에 접근했을 때에 hello world를 출력하게 해보려 합니다.

```python
from flask import Flask
from flask_restful import Resource, Api
```

flask와 flask_restful에서 필요한 것들을 가져옵니다.

```python
app = Flask(__name__)
api = Api(app)
```

Flask 객체를 생성합니다. 

그런 다음에 flask_restful의 메인 기본 진입점을 설정하고, Flask 객체로 초기화 합니다.

```python
class HelloWorld(Resource):
    def get(self):
        return {'hello': 'world'}
```

HelloWorld라는 클래스를 만듭니다.

이는 get하면 ```{'hello': 'world'}```가 반환됩니다.

```python
api.add_resource(HelloWorld, '/')
```

api에 대한 리소스 클래스를 추가하고, 라우터를 설정합니다.

```python
if __name__ == '__main__':
    app.run(debug=True)
```

이제 메인 함수에서 로컬 개발 서버를 구동할 수 있습니다.

## put get delete

put, get 그리고 delete를 할 수 있게 추가해보려 합니다.

```python
from flask import Flask, request
from flask_restful import Resource, Api
```

flask와 flask_restful에서 필요한 것들을 가져옵니다.

```python
app = Flask(__name__)
api = Api(app)
```

Flask 객체를 생성합니다. 

그런 다음에 flask_restful의 메인 기본 진입점을 설정하고, Flask 객체로 초기화 합니다.

```python
todos = {}
```

todos라는 딕셔너리에 put, delete를 시도하고, get으로 내용을 확인해보려 합니다.

```python
class HelloWorld(Resource):
    def get(self):
        return {'hello': 'world'}
```

```python
api.add_resource(HelloWorld, '/')
```

우선 루트 라우터에 접근하면 ```{'hello': 'world'}```가 출력되게 합니다.

```python
class Todo(Resource):
    def get(self, todo_id):
        return {todo_id: todos[todo_id]}
```

get은 딕셔너리에서 원하는 인덱스의 내용이 반환됩니다.

```python
    def put(self, todo_id):
        todos[todo_id] = request.form['data']
        return {todo_id: todos[todo_id]}
```

put은 데이터를 삽입하고 내용을 반환합니다.

```python
    def delete(self, todo_id):
        del todos[todo_id]
        return '', 204
```

delete는 내용을 지우고 status를 204로 반환합니다.

```python
api.add_resource(Todo, '/<int:todo_id>')
```

api에 대한 위 리소스 클래스를 추가하고, 라우터를 설정합니다.

```python
if __name__ == '__main__':
    app.run(debug=True)
```

다시 메인 함수에서 로컬 개발 서버를 구동할 수 있습니다.
