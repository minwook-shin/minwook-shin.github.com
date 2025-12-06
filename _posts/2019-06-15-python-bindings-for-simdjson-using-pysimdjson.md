---
layout: post
title: Python 대용량의 json 파싱하는 pysimdjson 라이브러리 알아보기
---

오늘은 Python으로 기가바이트 단위의 큰 json 파일을 파싱하는 simdjson을 바인딩한 pysimdjson을 알아보려 합니다.

## pysimdjson 설치

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
pip install pysimdjson
```

pip로 pysimdjson를 설치합니다.

## sample json

해당 패키지가 json을 파싱하는 역할을 하기 때문에 샘플 파일을 만듭니다.

아래 예시는 github event api의 json 코드를 일부 변형해보았습니다.

생략된 필드도 있고, 가상의 사용자와 저장소를 넣었습니다.

```json
{
  "type": "PushEvent",
  "created_at": "2019-01-01T07:58:30Z",
  "id": "0000000000",
  "actor": {
    "login": "test_id",
    "url": "https://api.github.com/users/test",
    "id": 1000000
  },
  "repo": {
    "url": "https://api.github.com/repos/test/helloworld",
    "id": 1000000,
    "name": "test/helloworld"
  },
  "public": true,
  "payload": {
    "commits": [
      {
        "url": "https://api.github.com/repos/test/helloworld/commits/05570a3080693f6e55244e012b3b1ec59516c01b",
        "message": "message",
        "distinct": true,
        "sha": "05570a3080693f6e55244e012b3b1ec59516c01b",
        "author": {
          "email": "test@example.com",
          "name": "test_name"
        }
      }
    ],
    "distinct_size": 1,
    "ref": "refs/heads/issue-1",
    "push_id": 1000000,
    "size": 1
  }
}
```

## loads

```python
import simdjson
```

simdjson을 가져옵니다.

```python
with open('sample.json', 'rb') as f:
    document = simdjson.loads(f.read())
```

위 json 문자열을 sample.json으로 저장하고 해당 파이썬 파일로 열어서 simdjson.loads 함수를 사용합니다.

딕셔너리로 반환됩니다.

```python
print(document)

print(document["type"])
print(document["created_at"])
print(document["id"])

print(document["actor"])
for k, v in document["actor"].items():
    print(k, v)

print(document["repo"])
for k, v in document["repo"].items():
    print(k, v)

print(document["public"])

print(document["payload"])
for k, v in document["payload"].items():
    print(k, v)
```

딕셔너리로 반환되므로 위와 같이 출력할 수 있습니다.

## query

```python
import simdjson
```

simdjson 패키지를 가져옵니다.

```python
with open('sample.json', 'rb') as fin:
    pj = simdjson.ParsedJson(fin.read())
```

simdjson.ParsedJson 함수로 simdjson.csimdjson.ParsedJson 타입의 변수를 반환됩니다.

JSON 문서를 파싱하는 경우 한 번만 open()을 수행하고 사용할 수 있습니다.

```python
    print(pj.items('.type'))
    print(pj.items('.created_at'))
    print(pj.items('.id'))
    print(pj.items('.actor[]'))
    print(pj.items('.actor.login'))
    print(pj.items('.repo.name'))
    print(pj.items('.public'))
    print(pj.items('.repo.name'))
    print(pj.items('.payload[]'))
    print(pj.items('.payload.commits[].author'))
```

위와 같이 출력할 수 있습니다.