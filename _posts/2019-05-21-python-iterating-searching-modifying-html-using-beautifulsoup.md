---
layout: post
title: Python HTML, XML을 파싱하는 BeautifulSoup 라이브러리 알아보기
---

오늘은 Python으로 HTML, XML을 파싱해서 원하는 작업을 할 수 있는 라이브러리인 BeautifulSoup을 적용해보려 합니다.

## BeautifulSoup 설치

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
pip install beautifulsoup4
```

pip로 beautifulsoup4를 설치합니다.

## hello world

짧은 예제를 만들어보려 합니다.

```python
from bs4 import BeautifulSoup
```

BeautifulSoup를 가져옵니다.

```python
html_content = """
<html><head><title>html title</title></head>
<body>
<p class="title"><b>hello, world!</b></p>

<p class="story">this is first story</p>
<a href="http://example.com" id="link1">test</a>,
<a href="http://example.com" id="link2">test</a>,
<p class="story">this is second story</p>
"""

soup = BeautifulSoup(html_content, 'html.parser')
```

html을 파싱해볼 수 있습니다.

```python
print(soup.prettify())
```

html 태그들을 보기 좋게 출력합니다.

이 때는 유실된 닫힌 태그들도 같이 출력됩니다.

```python
print("title:", soup.title)

print("title name:", soup.title.name)

print("title string:", soup.title.string)

print("title parent:", soup.title.parent.name)
```

BeautifulSoup 객체로 파싱된 html에서 필드를 출력할 수 있습니다.

```python
print("p:", soup.p)

print("p class:", soup.p['class'])
```

class 이름도 출력할 수 있습니다.

```python
print("a:", soup.a)

print("link1:", soup.find(id="link1"))
```

find로 태그와 매칭되는 첫 결과를 반환하여 출력합니다.

```python
print(soup.get_text())
```

하위 문자열을 다 가지고 와서 출력합니다.

```python
for url in soup.find_all('a'):
    print(url)
for url in soup.find_all('a'):
    print(url.get("href"))
```

모든 a 태그를 가져와서 해당 태그의 특정 속성의 내용만 가져올 수 있습니다.

## google search

구글 검색의 결과를 urllib로 가져와서 beautifulsoup으로 html을 파싱해보려 합니다.

```python
from urllib.request import Request, urlopen
from bs4 import BeautifulSoup
```

구글 사이트를 가져오기 위한 urllib와 파싱을 위한 BeautifulSoup을 가져옵니다.

```python
header = {'User-Agent': 'Mozilla/5.0'}
```

구글에서 크롤링을 하기 위해서 헤더를 지정합니다.

```python
word = input()
html = Request('https://www.google.com/search?q='+str(word), headers=header)
```

사용자가 입력한 단어를 검색 쿼리에 넣고 헤더를 첨부하여 Request객체를 만듭니다.

```python
source = urlopen(html)
soup = BeautifulSoup(source, "html.parser")
```

Request 객체를 urlopen으로 가져와서 html을 파싱합니다.

```python
for search in soup.find_all("div", id="search"):
    for article in search.find_all("h3"):
        print(article.find('a').text)
```

search라는 아이디를 가진 div 태그를 모두 찾아서 사이트 제목이 들어있는 h3 태그를 가져옵니다.

해당 h3 태그에서 링크가 첨부된 a 태그의 텍스트를 출력하게 됩니다.

최종적으로 첫번째 검색 결과의 제목이 터미널에 순서대로 출력됩니다.
