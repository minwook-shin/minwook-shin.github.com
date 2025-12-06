---
layout: post
title: Python 데이터 역직렬화하고 유효성 검사하는 Colander 라이브러리 알아보기
---

오늘은 Python으로 데이터를 직렬화하고 데이터의 유효성을 검증할 수 있는 Colander 패키지에 대하여 알아보려 합니다.

## Colander 설치

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
pip install colander
```

pip로 colander를 설치합니다.

## 실습

```python
import colander
```

colander를 가져옵니다.

```python
class Place(colander.MappingSchema):
    location = colander.SchemaNode(colander.String(),
                                   validator=colander.OneOf(['home', 'work']))
    Latitude_longitude = colander.SchemaNode(colander.String())
```

두 필드를 가진 Place라는 MappingSchema 클래스를 만듭니다.

location은 문자열이며 home이나 work중에 하나이어야 합니다.

Latitude_longitude는 문자열이어야 합니다.

```python
class GPS(colander.SequenceSchema):
    place = Place()
```

Place 객체를 할당하여 SequenceSchema인 GPS 클래스를 만듭니다.

```python
class Information(colander.MappingSchema):
    id = colander.SchemaNode(colander.String())
    login = colander.SchemaNode(colander.Int(), validator=colander.Range(0, 999))
    gps = GPS()
```

Information이라는 MappingSchema 클래스를 만듭니다.

해당 클래스는 id, login 필드와 SequenceSchema인 gps 필드로 구성되어 있습니다.

```python
my_information = {
    'id': 'example@example.com',
    'login': '1',
    'gps': [{'location': 'home', 'Latitude_longitude': '37.576108, 126.976838'},
            {'location': 'work', 'Latitude_longitude': '37.400527, 127.104773'}],
}
```

이제 유효성을 검사할 데이터 셋을 가져옵니다.

```python
schema = Information()
```

Information 객체를 스키마로 설정합니다.

```python
try:
    deserialized = schema.deserialize(my_information)
except colander.Invalid as e:
    print(e)
```

스키마를 역직렬화하여 객체를 만들어봅니다.

이 때에 데이터가 유효하지 않다면 except에 의해 출력됩니다.

```python
serialized = schema.serialize(my_information)
print(serialized)
```

다시 직렬화하여 출력합니다.

## imperative mode

```python
import colander
```

기존 클래스로 구성하는 것처럼 colander를 가져옵니다.

```python
schema = colander.SchemaNode(colander.Mapping())
schema.add(colander.SchemaNode(colander.String(), name='id'))
schema.add(colander.SchemaNode(colander.Int(), name='login',
                               validator=colander.Range(0, 999)))
```

스키마를 선언하듯이 구성할 수 있습니다.

각자의 SchemaNode를 추가하여 완성합니다.

```python
my_information = {
    'id': 'example@example.com',
    'login': '1',
}
```

유효성을 검사할 데이터 셋을 가져옵니다.

```python
try:
    deserialized = schema.deserialize(my_information)
except colander.Invalid as e:
    print(e)
```

스키마를 역직렬화하여 객체를 만들어봅니다.

이 때에 데이터가 유효하지 않다면 except에 의해 출력됩니다.

```python
serialized = schema.serialize(my_information)
print(serialized)
```

다시 직렬화하여 출력합니다.
