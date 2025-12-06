---
layout: post
title: Python YAML 문법을 파싱하는 PyYAML 라이브러리 알아보기
---

오늘은 Python으로 YAML 문법을 파싱해서 변수로 변환하게 해주는 PyYAML 패키지를 알아보려 합니다.

## PyYAML 설치

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
pip install pyyaml
```

pip로 PyYAML을 설치합니다.

## 예제

```python
import yaml
```

yaml 패키지를 가져옵니다.

```python
test = yaml.load("""
- first
- second
- third
""")

print(test)
```

yaml 형식으로 작성된 텍스트를 불러옵니다.

```python
yaml.warnings({'YAMLLoadWarning': False})
```

출력창에 나오는 경고를 지울 수 있습니다.

```python
test = yaml.load("""
- first
- second
- third
""", Loader=yaml.FullLoader)

print(test)
```

아니면 경고를 없애기 위해 full YAML language를 로드해서 불러와도 됩니다.

```python
doc = """
---
name: hello world
description: >
    testing file
---
name: readme
description: >
    readme file
---
name: make
description: >
    build file
"""

for data in yaml.load_all(doc):
    print(data)
```

각 블럭들을 모두 로드하면 for문으로 분리하여 출력할 수 있습니다.

```python
test = yaml.load("""
none: [~, null]
bool: [true, false, on, off]
int: 10
float: 3.14
list: [string,string]
dict: {id: "test_id", count: 5}
""")
print(test)
```

불러올 때에 자동으로 파이썬 타입으로 변환됩니다.

```python
print(yaml.dump({'name': 'test_name', 'email': 'example@example.com', 'todo': ['post article', 'sql study']}))
```

딕셔너리로 덤프하면 yaml 형식의 텍스트로 변경되어 출력됩니다.

```python
class TestClass:
    def __init__(self, id_, name, count):
        self.id = id_
        self.name = name
        self.count = count

obj = yaml.load("""
!!python/object:__main__.TestClass
id: test_id
name: test_name
count: 1
""")
print(obj.id)
print(obj.name)
print(obj.count)
```

파이썬 객체로 저장할 수 있습니다.

```python
print(yaml.dump(TestClass(id_="test_id", name="test_name", count=1)))
```

반대로 객체를 덤프하면 yaml 텍스트로 출력할 수 있습니다.
