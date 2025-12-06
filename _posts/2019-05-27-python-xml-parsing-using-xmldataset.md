---
layout: post
title: Python XML 파싱하는 xmldataset 라이브러리 알아보기
---

오늘은 Python으로 xml 파싱하는 라이브러리인 xmldataset을 적용해보려 합니다.

## xmldataset 설치

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
pip install xmldataset
```

pip로 xmldataset을 설치합니다.

## 예제

```python
import xmldataset
```

xmldataset을 가져옵니다.

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

xmldataset으로 파싱할 xml 텍스트를 가져옵니다.

```python
profile = """
site
    blog
        number     = external_dataset:blog_information
        article
            id     = dataset:blog_article,prefix:blog_article_
            author = dataset:blog_article,prefix:blog_article_
            url    = dataset:blog_article,prefix:blog_article_
            publish_date  = dataset:blog_article,name:date,prefix:blog_article_
            __EXTERNAL_VALUE__ = blog_information:number:blog_article"""
```

xmldataset으로 가져올 형식을 지정할 프로필도 작성합니다.

트리 형식으로 작성하면서 dataset과 external_dataset을 지정합니다.

추가적으로 prefix로 접두사, name으로 이름을 지정할 수 있습니다.

\_\_EXTERNAL*VALUE*\_ 으로 article 레벨의 밖에 있는 외부 값을 가져올 수 있습니다.

```python
result = xmldataset.parse_using_profile(xml, profile)
```

준비한 xml 텍스트를 profile 형식대로 파싱하여 result에 딕셔너리 형태로 반환합니다.

```python
import pprint

pp = pprint.PrettyPrinter(indent=4).pprint

pp(result)
```

pprint으로 정리하여 출력하게 할 수 있습니다.

```python
print(type(result))
```

result의 타입을 확인해보면 딕셔너리라고 확인할 수 있습니다.
