---
layout: post
title: Python feed 파싱하는 feedparser 라이브러리 알아보기
---

오늘은 Python에서 feed를 파싱할 수 있는 feedparser 라이브러리를 알아보려 합니다.

## feedparser 설치

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
pip install feedparser
```

pip로 feedparser를 설치합니다.

## 로컬

```python
import feedparser
from pprint import pprint
```

feedparser와 정리하고 출력해주는 pprint를 가져옵니다.

```python
data = """<rss version="2.0">
<channel>
<title>test feed</title>
</channel>
</rss>"""
```

rss feed를 로컬 파일로 파싱할 수 있습니다.

```python
local_fees = feedparser.parse(data)
```

feedparser로 파싱합니다.

```python
pprint(local_fees["feed"])
pprint(local_fees.feed)
pprint(local_fees.feed.title)
```

title만 존재하므로 title만 가져옵니다.

```
{'title': 'test feed',
 'title_detail': {'base': '',
                  'language': None,
                  'type': 'text/plain',
                  'value': 'test feed'}}
```

```
'test feed'
```

위 두 문자열이 출력됩니다.

## url

```python
import feedparser
from pprint import pprint
```

feedparser와 정리하고 출력해주는 pprint를 가져옵니다.

```python
feed = feedparser.parse('feed:https://minwook-shin.github.io/feed')
```

rss feed를 url로 파싱할 수 있습니다.

```python
pprint(feed["feed"])
pprint(feed.feed)
```

feed 전체를 가져옵니다.

```python
pprint(feed["entries"])
pprint(feed.entries[0].title)
pprint(feed.entries[0].link)
pprint(feed.entries[0].description)
pprint(feed.entries[0].published)
pprint(feed.entries[0].published_parsed)
pprint(feed.entries[0].id)
```

entries 전체를 가져오거나, 제목, 주소, 설명과 같이 별도의 필드만 가져올 수 있습니다.