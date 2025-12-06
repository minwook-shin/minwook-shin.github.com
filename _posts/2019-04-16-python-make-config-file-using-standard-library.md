---
layout: post
title: Python ini 파일 읽고 쓰는 표준 라이브러리 알아보기
---

오늘은 Python으로 ini 설정 파일을 읽고 쓰게 할 수 있는 표준 라이브러리인 configparser 패키지에 대하여 알아보려 합니다.

## 개요

기본적으로 포함되어있는 파이썬 표준 라이브러리로서 ini 파일로 이루어진 설정 파일을 읽고 쓸 수 있습니다.

## 설정 파일 만들기

```python
import configparser
```

configparser를 가져옵니다.

```python
config = configparser.ConfigParser()
```

ConfigParser 객체를 만듭니다.

```python
config['example.org'] = {}
config['example.org']['User'] = 'user_name'
```

```python
config['example.com'] = {}
example = config['example.com']
example['ID'] = 'real_id'
example['Email'] = 'example@example.com'
```

색션을 만들어서 값을 넣을 수 있습니다.

```python
config['DEFAULT']['Agree'] = 'True'
```

기본 색션으로 값을 넣을 수 있습니다.

```python
config['DEFAULT'] = {'Title': 'hello, world!',
                     'Body': 'print(hello,world!)', 'Author': 'realName'}
```

딕셔너리로 추가할 수 있습니다.

```python
with open('config.ini', 'w') as configfile:
    config.write(configfile)
```

파일을 열고 지금까지 기록한 ConfigParser객체를 설정 파일로 씁니다.

```
[DEFAULT]
title = hello, world!
body = print(hello,world!)
author = realName

[example.org]
user = user_name

[example.com]
id = real_id
email = example@example.com
```

위와 같이 파일이 작성됩니다.

## 설정 파일 읽기

```python
import configparser
```

configparser를 가져옵니다.

```python
config = configparser.ConfigParser()
```

ConfigParser 객체를 만듭니다.

```python
config.read('config.ini')
sec = config.sections()
print(sec)
```

config.ini 파일을 열고 색션을 출력합니다.

```python
example = config['example.com']
ex1 = example['Id']
print(ex1)
ex2 = example['Email']
print(ex2)
```

```python
user = config['example.org']['User']
print(user)
email = config['example.com']['Email']
print(email)
```

특정 색션의 내용을 가져올 수 있습니다.

```python
title = config['DEFAULT']['Title']
print(title)
```

기본으로 값이 써져있는 것은 DEFAULT 키워드를 사용합니다.

```python
print('example.org' in config)
print('example.com' in config)
```

해당 색션이 존재하는 지에 따라 bool 타입으로 출력됩니다.

```python
for i in config['example.com']:
    print("for : ", i, " | ", config['example.com'][i])
```

for문으로 키 값을 가져와서 출력할 수 있습니다.

특정 색션을 지정해도 DEFAULT로 지정된 기본 값도 같이 출력됩니다.
