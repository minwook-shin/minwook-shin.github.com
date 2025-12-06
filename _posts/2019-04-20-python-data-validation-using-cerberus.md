---
layout: post
title: Python 데이터 검증 cerberus 라이브러리 알아보기
---

오늘은 Python으로 데이터의 유효성을 검증할 수 있는 cerberus 패키지에 대하여 알아보려 합니다.

## cerberus 설치

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
pip install cerberus
```

pip로 cerberus를 설치합니다.

## 실습

```python
from cerberus import Validator
```

cerberus에서 Validator를 가져옵니다.

```python
schema = {'id': {'type': 'string'}, 'login': {'type': 'integer', 'max': 999}}
v = Validator(schema)
```

스키마를 지정하면 검사할 수 있습니다.

위 코드에서는 id가 문자열이어야 하고, login은 정수형이면서 999이하로 설정되어야 합니다.

```python
document = {'id': 'real_id'}
result = v.validate(document)
print(result)
```

위 id는 문자열이므로 검사했을 때에 true가 출력됩니다.

```python
document = {'id': 10}
result = v.validate(document)
print(result)
```

위 id는 문자열이 아니므로 검사했을 때에 false가 출력됩니다.

```python
document = {'email': 'example@exmpale.com'}
result = v.validate(document)
print(result)
```

email이라는 항목은 없기 때문에 false라고 출력됩니다.

```python
document = {'id': 'real_id', 'login': 1000}
result = v.validate(document, schema)
print(result)
```

id는 문자열이지만, login의 max 값을 999로 설정했기 때문에 false라고 출력됩니다.

```python
error = v.errors
print(error)
```

errors에 'max value is 999'처럼 어떤 항목에서 오류가 발생하는지 포함되어 있습니다.

```python
document = {'id': 'real_id', 'hello': 'world'}
result = v.validate(document, schema)
print(result)
```

hello라는 새로운 항목으로 인하여 false라고 출력됩니다.

```python
error = v.errors
print(error)
```

새로운 항목은 'unknown field'로 분류됩니다.

```python
v.schema = {'pin': {'type': 'integer', 'coerce': int}}
v.validate({'pin': '0000'})
result_type = type(v.document['pin'])
print(result_type)
```

'coerce': int와 같이 작성하면 출력되는 타입이 해당 타입으로 변경됩니다.

해당 항목의 타입을 확인해보면 'int'로 출력됩니다.

```python
normalized_doc = v.normalized(document, schema)
document_type = type(normalized_doc['hello'])
print(document_type)
```

스키마의 지정된 규칙에 따라 정규화된 문서를 반환하게 할 수 있습니다.
