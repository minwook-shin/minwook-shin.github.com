---
layout: post
title: Python HTML 마크업을 안전하게 해주는 MarkupSafe 라이브러리 알아보기
---

오늘은 Python으로 HTML 마크업 문자열을 예상치 못하게 수행하는 것을 이스케이프 처리를 하면서 안전하게 해주는 라이브러리인 MarkupSafe를 적용해보려 합니다.

## MarkupSafe 설치

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
pip install -U MarkupSafe
```

pip로 MarkupSafe를 설치합니다.

## 예제

```python
from markupsafe import Markup, escape
```

markupsafe를 가져옵니다.

```python
escape_code1 = escape('<script>alert(hello world!);</script>')
print(escape_code1)
print(escape_code1.unescape())
```

```python
escape_code2 = Markup.escape('<script>alert(hello world!);</script>')
print(escape_code2)
print(escape_code2.unescape())
```

script 수행을 막기 위해서 이스케이프 처리를 할 수 있습니다.

unescape를 통하여 다시 되돌릴 수도 있습니다.

```python
markup_code1 = Markup('<script>alert(hello world!);</script>')
print(markup_code1)
```

이미 안전하다고 가정하고 html에 넣기 위한 준비를 마친 마크업을 만들 수 있습니다.

```python
template = Markup("Hello <em>%s</em>")
markup_code2 = template % '"World"'
print(markup_code2)
```

str의 서브 클래스이기 때문에 위와 같이 작성할 수 있습니다.

```python
class Image:
    def __init__(self, url):
        self.url = url

    def __html__(self):
        return '<img src="%s">' % self.url
```

클래스를 만들어서 \_\_html\_\_로 마크업 문자열을 반환할 수 있습니다.

```python
img = Image('logo.png')
img_code = Markup(img)
print(img_code)
```

`<img src="logo.png">`로 출력됩니다.

```python
class User:
    def __init__(self, id, name):
        self.id = id
        self.name = name

    def __html__(self):
        return '<a href="/user/{}">{}</a>'.format(
            self.id, escape(self.name)
        )
```

위에 작성한 클래스와 다른 점은 이름을 별도로 이스케이프 시켰다는 점입니다.

이와 같이 별도의 문자열만 이스케이프 처리를 할 수 있습니다.

```python
user = User(1, '<script>')
user_code = escape(user)
print(user_code)
```

script 태그를 이름으로 넣어도 이스케이프 처리되어 이름이 &lt;script&gt;로 출력되게 됩니다.
