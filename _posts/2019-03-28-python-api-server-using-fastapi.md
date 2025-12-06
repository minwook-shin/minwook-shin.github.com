---
layout: post
title: Python FastAPI 사용해보기
---

오늘은 깃허브 트랜드를 보던 도중에 알게 되었던, fastapi 라이브러리를 직접 사용해보려 합니다.

## 깃허브 url

https://github.com/tiangolo/fastapi

## 프로젝트 라이선스

```
This project is licensed under the terms of the MIT license.
```

MIT 라이선스로 이루어져 있습니다.

## 개요

FastAPI는 api를 만들기 위한 라이브러리입니다.

Python 3.6 이상에 존재하는 타입 힌트에 기반한 API 웹 프레임 워크입니다.

## 장점

Starlette, Pydantic 패키지 덕분에 노드나 Go언어에 기반한 라이브러리에 비슷하게 속도가 구현되있다고 합니다.

그리고 짧은 코드와 사용법이 쉽다고도 강조되어 있습니다.

## 요구 사항

Python 3.6+

## 설치

```
pip install fastapi
```

fastapi를 pip로 설치합니다.

```
pip install email-validator
```

email-validator가 설치되어있지 않으면 서버가 구동되지 않으므로 같이 설치해줍니다.

```
pip install uvicorn
```

서버 구동을 위한 uvicorn도 pip로 설치해줍니다.

## hello world

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
def read_root():
    return {"Hello": "World"}
```

FastAPI 객체를 생성해주고, 루트 경로를 get으로 가져오기로 할 함수를 만들어줍니다.

http://127.0.0.1:8000/ 에 접속하면 `{"Hello":"World"}`처럼 코드의 반환 값대로 출력됩니다.

## 서버 구동

```
uvicorn main:app --reload
```

파이썬의 모듈 이름과 FastAPI 객체 이름으로 uvicorn 서버를 구동합니다.

reload 옵션을 주면 코드가 변경될 때에 자동으로 반영됩니다.

## 심화

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/search/{search_id}")
def read_search(search_id: int, q: str = None):
    return {"search_id": search_id, "q": q}
```

Path Parameters에 의해서 search_id로 검색할 수 있습니다.

기본 값이 None으로 설정된 q는 옵션으로 검색할 수 있습니다.

http://127.0.0.1:8000/search/1?q=test 으로 접속해서 테스트할 수 있습니다.

## 비동기

```python
@app.get('/')
async def read_result():
    result = await some_func()
    return result
```

다른 비동기 함수에 의해 결과 값을 기다려야 한다면, async def로 처리할 수 있습니다.

## swagger ui

api 정보들을 사용자와 상호 작용할 수 있는 라이브러리로서 기본적으로 /docs에 접근하면 존재합니다.

http://127.0.0.1:8000/docs.

## ReDoc

api 레퍼런스 문서를 생성해주는 라이브러리로서 기본적으로 /redoc에 접근하면 존재합니다.

http://127.0.0.1:8000/redoc

## 배포

```
uvicorn main:app --host 0.0.0.0 --port 80
```

uvicorn으로 호스트와 포트를 지정하여 실제 웹에 배포할 수 있습니다.

```Docker
FROM tiangolo/uvicorn-gunicorn-fastapi:python3.7

COPY ./app /app
```

```
docker build -t myimage .
```

```
docker run -d --name mycontainer -p 80:80 myimage
```

또는 uvicorn-gunicorn-fastapi를 기반으로 하는 도커 파일을 만들어서 서버를 구동할 수 있습니다.

위 코드대로 수행하면 프로젝트 폴더의 /app 폴더에 존재하는 파일들로 myimage라는 이미지를 빌드되게 됩니다.

빌드된 이미지는 mycontainer라는 이름의 컨테이너로 80 포트에서 서버가 구동되게 됩니다.
