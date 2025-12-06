---
layout: post
title: Python 웹 메타 데이터를 가져오는 micawber 라이브러리 알아보기
---

오늘은 Python에서 링크에서 웹 메타 데이터를 가져와서 출력해주는 micawber 라이브러리를 알아보려 합니다.

## micawber 설치

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
pip install micawber
```

pip로 micawber를 설치합니다.

## embed

```python
import micawber
providers = micawber.bootstrap_basic()
embed_output = providers.parse_text('바로 보기 : \n https://www.youtube.com/watch?v=R6IT_f0XPT8')
print(embed_output)
```

유튜브 링크를 감지해서 embed 형식으로 변환할 수 있습니다.

```
바로 보기 : 
<iframe width="480" height="270" src="https://www.youtube.com/embed/R6IT_f0XPT8?feature=oembed" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
```

위와 같이 변환되어 출력됩니다.

## provider

```python
from micawber.providers import Provider, bootstrap_oembed
from pprint import pprint

youtube = Provider('http://www.youtube.com/oembed')
test = youtube.request('https://www.youtube.com/watch?v=R6IT_f0XPT8')
pprint(test)
```

Provider로 유튜브 주소를 지정하고 웹 컨텐츠를 요청하면 비디오의 제목부터 게시자까지 딕셔너리로 출력됩니다.

```python
providers = bootstrap_oembed()
```

bootstrap_oembed 객체를 생성합니다.

```python
"""
    {
        "provider_name": "YouTube",
        "provider_url": "https:\/\/www.youtube.com\/",
        "endpoints": [
            {
                "schemes": [
                    "https:\/\/*.youtube.com\/watch*",
                    "https:\/\/*.youtube.com\/v\/*",
                    "https:\/\/youtu.be\/*"
                ],
                "url": "https:\/\/www.youtube.com\/oembed",
                "discovery": true
            }
        ]
    },
"""
```

https://oembed.com/providers.json 에서 YouTube를 찾으면 위와 같습니다.

oEmbed에 의해 endpoints를 쉽게 알 수 있게 됩니다.

```python
test = providers.request('https://www.youtube.com/watch?v=R6IT_f0XPT8')
pprint(test)
```

그래서 providers로도 유튜브 주소에 존재하는 메타 데이터를 받을 수 있게 됩니다.