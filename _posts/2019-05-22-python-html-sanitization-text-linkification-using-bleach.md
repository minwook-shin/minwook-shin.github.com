---
layout: post
title: Python HTML 속성을 정리해주는 Bleach 라이브러리 알아보기
---

오늘은 Python으로 지정한 HTML 속성만 남기고 정리해주는 라이브러리인 Bleach를 적용해보려 합니다.

## Bleach 설치

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
pip install bleach
```

pip로 Bleach를 설치합니다.

## Sanitizing text

```python
import bleach
```

bleach를 가져옵니다.

```python
tag1 = bleach.clean('<b><i>example</i></b>', tags=['b'])
print(tag1)
```

특정 태그만 남기고 나머지는 태그가 실행되지 않도록 정리합니다.

```python
tag2 = bleach.clean('<p class="foo" style="color: red; font-weight: bold;">example</p>',
                    tags=['p'],
                    attributes=['style'],
                    styles=['color'])
print(tag2)
```

특정 태그, 속성, 스타일을 남기고 나머지는 태그가 실행되지 않도록 정리합니다.

```python
attrs = {'a': ['href', 'rel'],'img': ['alt']}

tag3 = bleach.clean('<img alt="an example" width=500>', tags=['img'], attributes=attrs)
print(tag3)
```

속성들을 딕셔너리로 묶어서 남기고 태그가 실행되지 않도록 정리합니다.

```python
def allow_h(_, name, __):
    return name[0] == 'h'


tag4 = bleach.clean('<a href="http://example.com" title="link">example</a>', tags=['a'], attributes=allow_h)
print(tag4)
```

h로 시작하는 속성의 이름을 남기고 태그가 실행되지 않도록 정리합니다.

```python
tag5 = bleach.clean('<a href="smb://more_text">example</a>',protocols=['http', 'https', 'smb'])
print(tag5)
```

특정 프로토콜만 남기고 태그가 실행되지 않도록 정리합니다.

```python
tag6 = bleach.clean('<span>example</span>', strip=True)
print(tag6)
```

정리할 태그를 삭제할 결정을 할 수 있습니다.

```python
html = '<!-- commented -->example'

tag7 = bleach.clean(html)
print(tag7)
```

주석을 지울 수 있습니다.

## Linkifying text

```python
import bleach
from bleach.linkifier import Linker
```

bleach를 가져옵니다.

```python
link1 = bleach.linkify('http://example.com example')
print(link1)
```

기본적으로 example.com에 대한 a 태그가 생성됩니다.

```python
def set_title(attrs, _):
    attrs[(None, 'title')] = 'example title'
    return attrs


linker = Linker(callbacks=[set_title])
link2 = linker.linkify('http://example.com example')
print(link2)
```

함수를 이용하여 a 태그의 타이틀 속성을 추가할 수 있습니다.

```python
def allowed_attrs(attrs, _):
    allowed = [
        (None, 'href'),
        (None, 'style'),
        '_text',
    ]
    return dict((k, v) for k, v in attrs.items() if k in allowed)


linker = Linker(callbacks=[allowed_attrs])
link3 = linker.linkify('<a style="font-weight: super bold;" href="http://example.com">example</a>')
print(link3)
```

함수를 이용하여 특정 속성만 허용할 수 있습니다.

```python
def shorten_url(attrs, _):
    text = attrs['_text']
    if len(text) > 20:
        attrs['_text'] = text[0:15] + '...'
        return attrs


linker = Linker(callbacks=[shorten_url])
link4 = linker.linkify('http://example.com/longlongurl')
print(link4)
```

함수를 이용하여 길이가 긴 url는 보여지는 문자열을 축약시킬 수 있습니다.

```python
def remove_http(attrs, _):
    if attrs[(None, 'href')].startswith('http:'):
        return None


linker = Linker(callbacks=[remove_http])
link5 = linker.linkify('<a href="http://example.com">example</a>')
print(link5)
```

함수를 이용하여 href 속성에 http로 시작하는 내용이 있으면 해당 속성을 제거할 수 있습니다.
