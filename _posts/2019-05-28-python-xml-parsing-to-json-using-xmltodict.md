---
layout: post
title: Python json 형태로 XML 파싱하는 xmltodict 라이브러리 알아보기
---

오늘은 Python으로 json 형태(OrderedDict)로 xml을 파싱하는 라이브러리인 xmltodict을 적용해보려 합니다.

## xmltodict 설치

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
pip install xmltodict
```

pip로 xmltodict을 설치합니다.

## 예제

```python
import json
import xmltodict
```

딕셔너리 객체를 json 형식의 문자열로 바꾸기 위한 json 패키지와 OrderedDict으로 XML 파싱하는 xmltodict패키지를 가져옵니다.

```python
xml = """<?xml version="1.0"?>
  <site>
     <blog number="1">
        <article id="https://minwook-shin.github.io/">
           <author>minwook-shin</author>
           <url>http://minwook-shin.github.io/python-parsing-serializing-html-documents-using-html5lib/</url>
           <publish_date>2019-05-24T00:00:00+00:00</publish_date>
        </article>
        <article id="https://minwook-shin.github.io/">
           <author>minwook-shin</author>
           <url>http://minwook-shin.github.io/python-parse-and-build-css-using-cssutils/</url>
           <publish_date>2019-05-23T00:00:00+00:00</publish_date>
        </article>
        <article id="https://minwook-shin.github.io/">
           <author>minwook-shin</author>
           <url>http://minwook-shin.github.io/python-html-sanitization-text-linkification-using-bleach/</url>
           <publish_date>2019-05-22T00:00:00+00:00</publish_date>
        </article>
        <article id="https://minwook-shin.github.io/">
           <author>minwook-shin</author>
           <url>http://minwook-shin.github.io/python-iterating-searching-modifying-html-using-beautifulsoup/</url>
           <publish_date>2019-05-21T00:00:00+00:00</publish_date>
        </article>
     </blog>
  </site>"""
```

이전 포스팅에서 사용한 파싱할 xml 텍스트를 가져옵니다.

```python
import pprint

pp = pprint.PrettyPrinter(indent=4).pprint

result = json.dumps(xmltodict.parse(xml))
pp(result)
```

pprint으로 json 형식의 문자열을 정리하여 출력하게 할 수 있습니다.

## 콜백

```python
def handle_function(_, blog):
    print(blog['url'])
    return True
```

딕셔너리의 아이템을 인자로 받아서 특정 키 값만 출력하게 할 수 있습니다.

```python
result = json.dumps(xmltodict.parse(xml, item_depth=3, item_callback=handle_function))
pp(result)
```

위와 같이 작성하면 url 필드만 출력되게 됩니다.

## 반환 형식

```python
print(type(xmltodict.parse(xml)))
```

타입은 collections.OrderedDict으로 출력되는 것을 확인할 수 있습니다.
