---
layout: post
title: Python HTML 문서를 파싱하고 직렬화하는 html5lib 라이브러리 알아보기
---

오늘은 Python으로 HTML 문서를 가져와서 파싱하고 직렬화할 수 있는 라이브러리인 html5lib를 적용해보려 합니다.

## html5lib 설치

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
pip install html5lib
```

pip로 html5lib를 설치합니다.

## 예제

```python
import html5lib
```

html5lib를 가져옵니다.

```python
document1 = html5lib.parse("<p>Hello World!</p>")
print(document1)
```

html을 파싱할 수 있습니다.

```python
from urllib.request import urlopen
```

인터넷 상의 html 문서를 가져오기 위해서 urllib.request의 urlopen을 가져옵니다.

```python
with urlopen("http://www.google.com/") as f:
    document2 = html5lib.parse(f, transport_encoding=f.info().get_content_charset())
    print(document2)
```

urlopen으로 구글 사이트의 코드를 가져와 html 파싱을 시도합니다.

Content-Type 헤더의 charset 파라미터를 파싱할 때에 이용합니다.

```python
document3 = html5lib.HTMLParser(tree=html5lib.getTreeBuilder("dom")).parse("<p>Hello World!</p>")
print(document3)
```

HTMLParser 객체를 tree 인자로 treebuilder 클래스를 넣어서 다른 문서 형식을 사용할 수 있습니다.

```python
element = html5lib.parse('<p>Hello World!</p>')
walker = html5lib.getTreeWalker("etree")
stream = walker(element)
s = html5lib.serializer.HTMLSerializer().serialize(stream)
for i in s:
    print(i)
```

다양한 트리 유형에 대한 TreeWalker에 파싱 객체를 넣어줍니다.

직렬화를 통해 iterator 형식의 html 코드를 출력할 수 있습니다.

```python
from html5lib.filters import sanitizer
```

html5lib.filters의 sanitizer를 가져옵니다.

```python
dom = html5lib.parse("<script>alert('warning!')</script>", treebuilder="dom")
walker = html5lib.getTreeWalker("dom")
clean = sanitizer.Filter(walker(dom))
print(clean)
```

sanitizer.Filter로 안전하지 않은 마크업과 CSS를 제거할 수 있습니다.
