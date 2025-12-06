---
layout: post
title: Python readability 도구로 사용하는 python-readability 라이브러리 알아보기
---

오늘은 Python에서 arc90의 readability 도구로 사용할 수 있는 python-readability 라이브러리를 알아보려 합니다.

## python-readability 설치

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
pip install readability-lxml
```

pip로 python-readability를 설치합니다.

## hello world 예제

hello world라는 문장을 위키피디아로 검색한 문서를 python-readability로 제목과 본문을 출력해보았습니다.

```python
import urllib.request
from readability import Document
```

해당 라이브러리에서 없는 기능인 웹에서 가져오는 urllib의 request와 readability를 가져옵니다.

```python
req = urllib.request.Request(
    'https://en.wikipedia.org/wiki/%22Hello,_World!%22_program',
    headers={
        'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_5) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/12.1.1 Safari/605.1.15'
    }
)
```

wikipedia에 있는 Hello,_World!_program 프로그램에 대해 가져오기 위해 url과 헤더에 User-Agent를 설정합니다.

```python
with urllib.request.urlopen(req) as f:
    urllib_content = f.read()
```

해당 Request를 비탕으로 웹 컨텐츠를 가져옵니다.

```python
print(urllib_content.decode("utf-8"))
```

해당 상태는 자바스크립트 코드와 같은 문자열이 포함되어 있어서 가독성이 안좋습니다.

```python
doc = Document(urllib_content)
```

Document 객체를 만듭니다.

```python
print(doc.short_title())
print(doc.title())
```

아래와 같이 제목을 추출할 수 있습니다.

"Hello, World!" program
"Hello, World!" program - Wikipedia

short_title과 title이 출력됩니다.

```python
print(doc.summary())
```

summary로 가독성 좋게 필요없는 태그는 없어지고, 읽을 수 있는 문자열의 html 태그만 남습니다.