---
layout: post
title: Python jquery 문법으로 HTML 파싱하는 PyQuery 라이브러리 알아보기
---

오늘은 Python으로 jquery 문법으로 HTML 파싱하는 라이브러리인 PyQuery를 적용해보려 합니다.

## PyQuery 설치

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
pip install pyquery
```

pip로 PyQuery를 설치합니다.

## 속성

```python
from pyquery import PyQuery

d = PyQuery("<option value='1'><option value='2'>")
print(d('option[value="1"]'))
```

PyQuery를 가져와서 특정 속성을 가져올 수 있습니다.

```python
p = PyQuery('<p id="hello" class="hello"></p>')
p.attr.id = "hello_world"
print(p)

p.attr["id"] = "hello_world"
print(p)

p.attr(id='hello', class_='hello2')
print(p)
```

PyQuery를 가져와서 특정 속성 필드에 값을 대입할 수 있습니다.

## css

```python
from pyquery import PyQuery

p = PyQuery('<p id="hello" class="hello"></p>')('p')
print(p.addClass("hello_world"))
print(p.toggleClass("python hello_world"))
print(p.removeClass("python"))
```

PyQuery를 가져와서 클래스를 추가하거나 변경, 또는 삭제할 수 있습니다.

```python
p = PyQuery('<p id="hello" class="hello"></p>')
p.css.font_size = "1px"
print(p.attr.style)
p.css['font-size'] = "2px"
print(p.attr.style)
p.css(font_size="3px")
print(p.attr.style)
p.css = {"font-size": "4px"}
print(p.attr.style)
```

PyQuery를 가져와서 css 속성 필드에 값을 대입할 수 있습니다.

## 의사 클래스 선택자

```python
from pyquery import PyQuery

d = PyQuery('<div><input type="button"/><button></button></div>')
print(d(':button'))

d = PyQuery('<div><input type="file"/><textarea></textarea></div>')
print(d(':input'))

d = PyQuery('<div><h1/><h1 class="title">title</h1></div>')
print(d('h1:contains("title")'))

d = PyQuery('<div><h1><span>title</span></h1><h1/></div>')
print(d('h1:parent'))

d = PyQuery('<div><p class="first"></p><p></p></div>')
print(d('p:first'))
print(d('p:last'))

d = PyQuery('<div><h1><span>title</span></h1><h2/></div>')
print(d(':empty'))

d = PyQuery('<div><h1>title</h1></div>')
print(d(':header'))

d = PyQuery('<div class="foo"><div class="bar"></div></div>')
print(d('.foo:has(div)'))
```

특정 부분과 상황만 선택할 수도 있습니다.

## 조작

```python
d = PyQuery('<p class="hello" id="hello">hello, world!</p>')
d('p').append(' check out <a href="https://www.google.com">google</a>')
print(d)
```

특정 태그의 내부에 추가할 수 있습니다.

```python
p = d('p')
d = PyQuery('<html><body><div id="test"><a href="https://github.com">github</a></div></body></html>')
p.prependTo(d('#test'))
print(d)
p.insertAfter(d('#test'))
print(d)
p.insertBefore(d('#test'))
print(d)
```

특정 html 텍스트를 기존의 html 앞 혹은 뒤에 넣을 수 있습니다.

```python
print(PyQuery('<div>hello,world!</div>').addClass('test') + PyQuery('<b>b</b>'))
```

연산자로 이어서 붙일 수 있습니다.

```python
d = PyQuery('<p id="hello" class="hello"><a/></p><p id="test"><a/></p>')
print(d('p').filter('.hello'))

print(d('p').find('a'))
print(d('p').eq(1).find('a'))

print(d('p').find('a').end())
```

특정 태그에 필터나 검색을 할 수 있습니다.
