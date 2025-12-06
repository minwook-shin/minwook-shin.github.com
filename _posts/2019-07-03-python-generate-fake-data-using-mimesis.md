---
layout: post
title: Python 가상 데이터 생성하는 Mimesis 라이브러리 알아보기
---

오늘은 Python에서 가상의 데이터를 무작위 생성해서 데이터베이스 테스트하거나 json, xml 형식의 파일을 만들 수 있는 Mimesis 라이브러리를 알아보려 합니다.

## Mimesis 설치

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
pip install mimesis 
```

pip로 Mimesis를 설치합니다.

## 사람 이름

```python
from mimesis import Person
from mimesis.enums import Gender
```

mimesis에서 Person과 Gender를 가져옵니다.

```python
person = Person('en')

print(person.full_name(gender=Gender.FEMALE))
print(person.full_name(gender=Gender.MALE))
```

Person으로 사람 이름을 무작위로 생성할 수 있습니다.

이 때에 인자로 언어를 다르게 설정할 수 있으며, gender에 의해 성별이 차이납니다.

## 언어 설정

```python
from mimesis import Generic
```

mimesis에서 Generic을 가져옵니다.

```python
generic = Generic('ko')

print(generic.datetime.month())
print(generic.code.imei())
print(generic.food.fruit())
```

언어를 설정하여 날짜나 음식등을 현지화하여 무작위 생성할 수 있습니다.

## 지역

```python
from mimesis import Address
```

mimesis에서 Address를 가져옵니다.

```python
address_ko = Address('ko')

print(address_ko.region())
print(address_ko.federal_subject())
print(address_ko.address())
print(address_ko.address())
```

주소를 각 언어에 맞게 무작위 생성할 수 있습니다.

## 시드

```python
from mimesis import Person
```

mimesis에서 Person을 가져옵니다.

```python
person = Person('ko', seed=0xffff)
print(person.full_name())
```

시드 값을 지정하여 무작위 값을 고정하여 출력할 수 있습니다.

## 필드

```python
from mimesis.schema import Field, Schema
```

mimesis에서 Field, Schema를 가져옵니다.

```python
field = Field('ko')
schema_dict = (lambda: {
    'version': field('version', pre_release=True),
    'timestamp': field('timestamp', posix=False),
    'token': field('token_hex'),
    'owner': {
        'id': field('uuid'),
        'email': field('person.email', key=str.lower)
    },
})
schema = Schema(schema=schema_dict)
print(schema.create(iterations=2))
```

스키마를 람다로 지정하고, 스키마에 전달하면 create()에 의해 호출됩니다.

iterations의 갯수에 따라 여러개를 생성할 수도 있습니다.