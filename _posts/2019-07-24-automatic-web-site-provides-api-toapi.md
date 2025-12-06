---
layout: post
title: Python 웹 사이트 api를 만들 수 있는 toapi 라이브러리 알아보기
---

오늘은 Python에서 웹 페이지의 선택자 경로를 통해 가져오는 문자와 링크로 웹 사이트 api를 만들 수 있는 toapi 라이브러리를 알아보려 합니다.

## toapi 설치

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
pip install toapi
```

pip로 toapi를 설치합니다.

```
pip install cssselect
```

추가로 cssselect도 설치합니다.

## 블로그 api 만들기 예제


```html
<article class="post">

    <h1><a href="/automatic-summarization-of-text-documents-sumy/">Python 자동 summarization sumy 라이브러리 알아보기</a></h1>

    <div class="entry">
    <p>오늘은 Python에서 자동으로 summarization 작업을 해줘서 텍스트와 html 코드를 요약하는 sumy 라이브러리를 알아보려 합니다.</p>
    </div>

    <a href="/automatic-summarization-of-text-documents-sumy/" class="read-more">Read More</a>
</article>
```

지금 보고 계시는 블로그의 메인 페이지에서 각 포스트마다 위 구조로 이루어져 있습니다.

해당 구조를 이용하여 웹 api를 만들어보려 합니다.

```python
from htmlparsing import Attr, Text
from toapi import Api, Item
```

htmlparsing과 toapi를 가져옵니다.

```python
api = Api()
```

Api 객체를 만듭니다.

```python
@api.site('https://minwook-shin.github.io')
@api.list('.post')
@api.route('/api', '/')
```

데코레이터로 사이트의 주소와 가져올 리스트의 선택자, 그리고 라우터를 설정합니다.

```python
class Post(Item):
    url = Attr('.read-more', 'href')
    title = Text('h1 > a')
```

반환할 항목을 선택자와 속성으로 지정합니다.

위와 같이 지정하면 

```
{
    "title": "How to solve the Codewars's Regex validate PIN code", 
    "url": "/how-to-solve-regex-validate-pin-code/"
}, 
```

위와 같이 출력됩니다.

```python
api.run(debug=True, host='0.0.0.0', port=5000)
```

이전에 만든 Api 객체를 가지고 서버를 구동합니다.