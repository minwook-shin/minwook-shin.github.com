---
layout: post
title: Python 객체에서 데이터 타입으로 바꾸는 marshmallow 라이브러리 알아보기
---

오늘은 Python으로 객체를 파이썬 기본 데이터 타입으로 변환하는 marshmallow를 알아보려 합니다.

## marshmallow 설치

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
pip install -U marshmallow
```

pip로 marshmallow를 설치합니다.

## serializing

```python
from marshmallow import Schema, fields, pprint
```

marshmallow에서 필요한 것들을 가져옵니다.

```python
class User(object):
    def __init__(self, name, email):
        self.name = name
        self.email = email
```

User 모델 클래스를 만듭니다.

해당 모델은 이름과 이메일 필드를 가집니다.

```python
class UserSchema(Schema):
    name = fields.Str()
    email = fields.Email()
```

위에서 만든 클래스의 속성 이름들에 Field 객체에 매핑되는 함수를 대입하여 스키마를 만듭니다.

```python
user = User(name="name", email="name@example.org")
schema = UserSchema()
```

User 객체와 스키마 객체를 생성합니다.

```python
result = schema.dump(user)
pprint(result.data)
```

스키마의 dump 메서드에 객체를 전달하여 serialize합니다

```python
result = schema.dumps(user)
pprint(result.data)
```

JSON으로 인코딩 된 문자열로 serialize 할 수도 있습니다.

## deserializing

```python
from marshmallow import Schema, fields, post_load, pprint
```

marshmallow에서 필요한 것들을 가져옵니다.

```python
class User(object):
    def __init__(self, name, email):
        self.name = name
        self.email = email
```

User 모델 클래스를 만듭니다.

해당 모델은 이름과 이메일 필드를 가집니다.

```python
class UserSchema(Schema):
    name = fields.Str()
    email = fields.Email()

    @post_load
    def make_user(self, data):
        return User(**data)
```

위에서 만든 클래스의 속성 이름들에 Field 객체에 매핑되는 함수를 대입하여 스키마를 만듭니다.

또한 post_load로 데코레이팅하면 deserialize한 값이 반환되게 됩니다.

결국 user 객체가 반환됩니다.

```python
user_data = {
    'email': u'name@exmple.com',
    'name': u'name'
}
```

이메일과 이름의 키 값을 가진 user_data 딕셔너리를 만듭니다.

```python
schema = UserSchema()
result = schema.load(user_data)
pprint(result.data)
```

스키마의 load 메서드에 객체를 전달되지만 최종적으로 post_load가 데코레이팅되어 deserialize하게 됩니다.

## vaildation

```python
from marshmallow import fields, Schema, validates, ValidationError, pprint
```

marshmallow에서 필요한 것들을 가져옵니다.

```python
class UserSchema(Schema):
    name = fields.Str(required=True)
    email = fields.Email()
```

위에서 만든 클래스의 속성 이름들에 Field 객체에 매핑되는 함수를 대입하여 스키마를 만듭니다.

단, 이번에는 name이라는 이름 필드가 반드시 들어가게 required 옵션을 True로 설정하였습니다.

```python
data, errors = UserSchema().load({'email': 'foo'})
print(data, errors)
```

이름 필드를 작성하지 않은 채로 틀린 이메일 형식만 쓰면 errors에서 Missing data for required field.와 Not a valid email address.가 동시에 담겨 출력됩니다.