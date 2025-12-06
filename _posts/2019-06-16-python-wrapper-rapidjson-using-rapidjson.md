---
layout: post
title: Python json 파싱하고 serialization 하는 python-rapidjson 라이브러리 알아보기
---

오늘은 Python으로 json 파일을 파싱하고 serialization 해주는 python-rapidjson을 알아보려 합니다.

## python-rapidjson 설치

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
pip install python-rapidjson
```

pip로 python-rapidjson를 설치합니다.

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

## 예제

```python
import rapidjson
```

rapidjson을 가져옵니다.

```python
with open("sample.json", "rb") as f:
    sample = rapidjson.loads(f.read())

    print(sample)
    print(type(sample))
```

위에서 만든 sample.json을 열어서 rapidjson.loads로 불러오면 딕셔너리 형태로 반환됩니다.

```python
print(rapidjson.dumps(sample))
print(type(rapidjson.dumps(sample)))
```

rapidjson.dumps를 수행하면 문자열로 변환됩니다.

```python
class Stream:

    def write(self, data):
        print("Chunk:", data)
```

```python
rapidjson.dump(sample, Stream(), chunk_size=100)
```

dump할 때에 클래스를 만들어서 chunk의 크기를 지정하여 나눠 처리할 수 있습니다.
