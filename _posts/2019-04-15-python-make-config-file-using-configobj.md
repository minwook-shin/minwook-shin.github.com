---
layout: post
title: Python Config 파일 읽고 쓰는 ConfigObj 패키지 알아보기
---

오늘은 Python으로 설정 파일을 읽고 쓰게 할 수 있는 ConfigObj 패키지에 대하여 알아보려 합니다.

## 개요

ini 파일로 이루어진 설정 파일을 읽고 쓸 수 있으며, 파이썬의 배열 인덱스처럼 사용할 수 있습니다.

## ConfigObj 설치

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
pip install configobj
```

pip로 ConfigObj를 설치합니다.

## 설정 파일 만들기

```python
from configobj import ConfigObj
```

설정 파일을 파싱하거나 쓰는 역할을 하는 ConfigObj를 가져옵니다.

```python
config = ConfigObj()
config.filename = "config.ini"
```

ConfigObj 객체를 만들고, 파일 이름을 정의합니다.

```python
config['name'] = "name"
config['nickname'] = "nick_name"

config['USER'] = {}
config['USER']['id'] = "real_id"
config['USER']['email'] = "example@example.com"
```

```python
config['visitor'] = {}
config['visitor']['today'] = ["1", "2", "3", "4", "5", "6", "7"]
```

인덱스 접근하는 방식처럼 설정 파일을 구성할 수 있습니다.

```python
blog = {
    'title': "hello,world",
    'body': "print(hello,world)",
    'tag': {
        'hello': "world"
    }
}
config['blog'] = blog
```

딕셔너리로 설정 파일을 구성할 수 있습니다.

```python
config.write()
```

지금까지 기록한 ConfigObj를 설정 파일로 씁니다.

```
name = name
nickname = nick_name
[USER]
id = real_id
email = example@example.com
[visitor]
today = 1, 2, 3, 4, 5, 6, 7
[blog]
title = "hello, world!"
body = "print(hello,world)"
[[tag]]
hello = world

```

위와 같이 파일이 작성됩니다.

## 설정 파일 읽기

```python
from configobj import ConfigObj
```

설정 파일을 파싱하거나 쓰는 역할을 하는 ConfigObj를 가져옵니다.

```python
config = ConfigObj("config.ini")
```

ConfigObj 객체의 인자로 읽을 파일 이름을 작성합니다.

```python
name = config['name']
print(name)
nick = config['nickname']
print(nick)

user = config['USER']
user_id = user['id']
user_email = user['email']
print(user)

title = config['blog']['title']
print(title)
body = config['blog']['body']
print(body)

print(config['blog']['tag'])

print(config['visitor'])
```

마찬가지로 설정 파일을 읽을 때에도 인덱스 접근하는 방식처럼 설정 파일을 구성할 수 있습니다.
