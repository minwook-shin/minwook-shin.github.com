---
layout: post
title: python urllib3로 url에서 데이터 받아오기
---

오늘은 python의 urllib3로 원하는 url에서 데이터를 받아오려 합니다.

먼저 json 서버를 만들기 위해 json-server를 사용합니다.

## 더미 json 데이터

더미 json 데이터를 서버에 올리기 위해서는 node 패키지가 필요합니다.

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

그리고 마지막으로 json-server를 전역으로 설치해주면 됩니다.

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

본인이 생각해둔 json 데이터의 구조를 위와 같이 파일로 저장합니다.

위 예시는 /user 라는 서브 라우터에 id가 0과 1인 두 데이터가 존재합니다.

```
json-server --watch db.json
```

db.json 파일로 이름을 저장하고, json-server을 구동해주면 가짜 REST API 로컬 서버로 만들 수 있습니다.

```
http://localhost:3000/user/0
```

서버를 구동하고 위 주소에 접근할 수 있으면, 이 주소를 가지고 python 코드에 사용할 수 있습니다.

## python urllib3

```python
import urllib3
from json import loads
from typing import Dict
```

url에서 데이터를 가져오기 위한 urllib3 패키지와 json 패키지의 loads를 통해 가져온 데이터가 json일 경우 dict 타입으로 바꾸어주게 합니다.

```python
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
```

InsecureRequestWarning: Unverified HTTPS request is being made. Adding certificate verification is strongly advised. 라면서 오류가 출력되면 코드에 위와 같이 작성해줘야 합니다.

```python
def getData(url: str) -> Dict:
    http = urllib3.PoolManager()
    req = http.request('GET', url)
    data = req.data
```

url을 문자열로 받고 json 패키지의 loads에 의해 dict 타입으로 반환되는 getData 함수를 만들어줍니다.

PoolManager 객체를 만들어서 request()로 HTTPResponse 객체를 반환하게 합니다.

이 HTTPResponse의 data에 우리가 원하는 데이터가 있습니다.

```python
    if req.status == 200:
        source: Dict = loads(req.data.decode('utf-8'))
        return source
    else:
        return None
```

HTTPResponse의 status에는 정상적으로 통신했다면 200이 포함되어 있습니다.

json 패키지의 loads로 json 형식 데이터를 가공해서 dict 타입 변수에 대입하고 반환합니다.

```python
get = getData("http://localhost:3000/user/0")
print(get)
```

getData 함수를 수행해서 get에는 dict 타입의 데이터가 담기게 됩니다.

```python
print(type(get) is dict)
```

print로 출력하면 dictd이 맞으므로 true가 출력됩니다.
