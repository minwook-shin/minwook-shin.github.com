---
layout: post
title: Node로 가짜 REST API 로컬 서버 구동하고, Python의 aiohttp로 json 데이터 받아오기
---

오늘은 node로 구동되는 json-server로 가짜 REST API 서버를 만들고, 이를 이용하여 Python의 aiohttp 패키지로 json 데이터를 가져오는 실습을 하려고 합니다.

## 더미 json 데이터 만들기

우선 더미 json 데이터를 서버에 올리기 위해서는 node 패키지가 필요합니다.

```
https://nodejs.org/ko/download/
```

윈도우의 경우에는 위 공식 사이트에서 내려받을 수 있습니다.

```
brew install node
```

맥의 경우에는 homebrew로 설치할 수 있습니다.

```
curl -sL https://deb.nodesource.com/setup_11.x | sudo -E bash -
sudo apt-get install -y nodejs
```

리눅스의 경우에는 curl을 이용하여 11버전의 노드를 설치할 수 있습니다.

```
npm install -g json-server
```

그리고 npm으로 json-server를 전역으로 설치해주면 됩니다.

```json
{
  "user": [
    {
      "id": 0,
      "score": 100
    },
    {
      "id": 1,
      "score": 99
    }
  ]
}
```

본인이 생각해둔 json 데이터의 구조를 위와 같이 나타내줍니다.

위 예시는 /user 라는 서브 라우터에 id가 0과 1인 두 데이터가 존재합니다.

```
http://localhost:3000/user?id=0
```

`` `[` `]` ``으로 감싸면 위와 같이 물음표로 세부적인 접근이 가능합니다.

그 외에도 정렬 및 나누기도 가능합니다.

```
json-server --watch db.json
```

db.json 파일로 이름을 저장하고, json-server을 구동해주면 가짜 REST API 로컬 서버를 만들 수 있습니다.

## python으로 데이터 받기

```
http://localhost:3000/user?id=0
```

준비해둔 api 서버를 이용하여 위와 같은 경로의 json 데이터를 받으려고 합니다.

```python
import aiohttp
import asyncio
import json
from typing import List, Tuple
```

http에서 받아오는 데이터 처리와 비동기, 그리고 json을 파이썬 타입으로 변환하기 위한 json 패키지를 준비합니다.

그리고 타입 명시를 위해 typing 패키지에서 List, Tuple을 가져옵니다.

```python
loop = asyncio.get_event_loop()
```

이벤트 루프를 생성해줍니다.

```python
async def get(session, url: str) -> Tuple:
    async with session.get(url) as response:
        return (await response.text(), response.status)
```

async로 비동기 함수를 만들어주고, 파이썬 3.6부터 도입된 타입 힌트로 함수의 반환값은 Tuple로 명시해줍니다.

반환 값이 여러개여서 튜플로 처리했습니다.

이 함수로 인해 아래에서 url을 넣으면 비동기적으로 데이터를 받게 됩니다.

```python
async def getHttpData() -> List:
    async with aiohttp.ClientSession() as session:
        source: Tuple = await get(session, 'http://localhost:3000/user?id=0')
        dictSource: List = json.loads(source[0])
        return dictSource
```

이 함수도 비동기 함수이며, List 타입으로 반환됩니다.

이전의 get 함수로 인하여 source에 들어가게 되면, 첫번째 값이 우리가 원하는 값이므로 이를 json.loads를 사용하여 파이썬 타입(List)으로 변환해줍니다.

변환된 채로 update에 의해 기존의 내용에서 status에 대한 값을 추가해보았습니다.

```python
result: List = loop.run_until_complete(getHttpData())
print(result)
```

getHttpData()로 반환이 다 될때까지 기다리고 출력됩니다.

```json
[{ "id": 0, "score": 100 }]
```

결과물이 나오게 됩니다.
