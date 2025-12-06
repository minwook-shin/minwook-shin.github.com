---
layout: post
title: Python URL 클래스 purl 라이브러리 알아보기
---

오늘은 Python에서 URL 클래스를 구현한 purl 라이브러리를 알아보려 합니다.

## purl 설치

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
pip install purl
```

pip로 purl을 설치합니다.

## 예제

```python
from purl import URL
```

purl을 가져옵니다.

```python
str_url = URL('https://www.google.com/search?q=google')
print(str_url)
print(str_url.as_string())

argument_url = URL(scheme='https', host='www.google.com', path='/search', query='q=google')
print(argument_url)
print(argument_url.as_string())

inline_url = URL().scheme('https').domain('www.google.com').path('search').query_param('q', 'google')
print(inline_url)
print(inline_url.as_string())
```

문자열, 인자, 인라인으로 url 객체를 생성할 수 있습니다.

```python
u = URL('postgres://username:password@localhost:1234/test?ssl=true')
print(u.scheme())
print(u.host())
print(u.domain())
print(u.username())
print(u.password())
print(u.netloc())
print(u.port())
print(u.path())
print(u.query())
print(u.path_segments())
print(u.query_param('ssl'))
print(u.query_param('ssl', as_list=True))
print(u.query_params())
print(u.has_query_param('ssl'))
print(u.subdomains())
```

데이터베이스 url도 파싱해서 스키마, 호스트 그리고 사용자 이름과 비밀번호와 같은 정보를 분리해서 출력할 수 있습니다.

```python
u = URL.from_string('https://github.com/minwook-shin')
print(u.path_segment(0))
```

인덱스에 맞는 segment를 출력합니다.

```python
new_url = u.add_path_segment('minwook-shin.github.com')
print(new_url.as_string())
```

경로에 segment를 추가합니다.

```python
from purl import expand

print(expand(u"{/path*}", {'path': ['sub', 'index']}))
```

expand 함수로 url 템플릿을 만들 수 있습니다.

```python
from purl import Template

template = Template("http://example.com{/path*}")
url = template.expand({'path': ['sub', 'index']})
print(url.as_string())
```

Template 객체로도 url 템플릿을 만들 수 있습니다.