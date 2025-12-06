---
layout: post
title: Python 데이터 유효성 검사하는 Schematics 라이브러리 알아보기
---

오늘은 Python으로 데이터의 유효성을 검증할 수 있는 Schematics 패키지에 대하여 알아보려 합니다.

## Schematics 설치

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
pip install schematics
```

pip로 Schematics를 설치합니다.

## 실습

```python
from schematics.models import Model
```

schematics에 Model 클래스를 만들 것을 가져옵니다.

```python
from schematics.transforms import blacklist
```

필드를 제외하기 위한 blacklist를 가져옵니다.

```python
from schematics.types import StringType, URLType
```

문자열, url 타입을 표시하기 위해 StringType, URLType을 가져옵니다.

```python
from schematics.exceptions import DataError
```

오류를 try except에서 잡기 위해서 DataError를 가져옵니다.

```python
import json
```

json 형식으로 출력하기 위해서 가져옵니다.

```python
class TestClass(Model):
    uuid = StringType(required=True)
    hello = StringType(required=True)
    url = URLType()
```

TestClass라는 Model 클래스를 만듭니다.

해당 모델에는 uuid와 hello가 필수로 요구되고, url은 url 형식으로만 작성되어야 합니다.

```python
    class Options:
        roles = {'public': blacklist('uuid')}
        serialize_when_none = False
```

모델에서 옵션을 설정할 수 있습니다.

룰을 지정하여 필드를 제외할 수 있고, 필수로 작성안해도 되는 필드에 값을 지정하지 않아서 none 처리되는 것을 지울 수도 있습니다.

```python
mock1 = TestClass.get_mock_object().to_primitive()
print(mock1)
```

mock 객체를 만들 수 있습니다.

```python
mock2 = TestClass.get_mock_object().to_primitive(role='public')
print(mock2)
```

룰이 적용된 mock 객체를 만들 수 있습니다.

```python
example1 = TestClass({'hello': 'world', 'url': 'https://www.python.org', 'uuid': '0000'})

json = json.dumps(example1.to_primitive())
print(json)
```

json으로 인코딩해서 출력할 수 있습니다.

```python
example2 = TestClass()
example2.uuid = '0000'
example2.hello = 'world'
example2.website = 'https://www.python.org'
```

값을 대입합니다.

```python
print(example2.uuid)
print(example2.hello)
print(example2.website)
```

필드 별로 출력할 수 있습니다.

```python
try:
    example2.validate()
except DataError as e:
    print(e)
```

만약 필드에 값이 대입되지 않았으면, DataError가 발생됩니다.
