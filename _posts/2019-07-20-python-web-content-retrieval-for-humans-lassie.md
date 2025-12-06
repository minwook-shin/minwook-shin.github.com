---
layout: post
title: Python 웹 컨텐츠를 보기 좋게 가져오는 Lassie 라이브러리 알아보기
---

오늘은 Python에서 웹 소스코드를 보기 좋게 가져와주는 Lassie 라이브러리를 알아보려 합니다.

## Lassie 설치

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
pip install lassie
```

pip로 Lassie를 설치합니다.

## 예제

```python
import lassie
from pprint import pprint

sample = lassie.fetch('https://www.youtube.com/watch?v=R6IT_f0XPT8')
pprint(sample)
```

lassie로 웹 컨텐츠를 가져와서 pprint로 출력하면 보기 좋게 출력합니다.

```python
{'images': [{'height': 360,
             'src': 'https://i.ytimg.com/vi/R6IT_f0XPT8/hqdefault.jpg',
             'width': 480},
            {'src': 'https://s.ytimg.com/yts/img/favicon-vfl8qSV2F.ico',
             'type': 'favicon'},
            {'src': 'https://s.ytimg.com/yts/img/favicon_32-vflOogEID.png',
             'type': 'favicon'},
            {'src': 'https://s.ytimg.com/yts/img/favicon_48-vflVjB_Qk.png',
             'type': 'favicon'},
            {'src': 'https://s.ytimg.com/yts/img/favicon_96-vflW9Ec0w.png',
             'type': 'favicon'},
            {'src': 'https://s.ytimg.com/yts/img/favicon_144-vfliLAfaB.png',
             'type': 'favicon'}],
 'locale': 'ko_KR',
 'oembed': {'author_name': '백종원의 요리비책',
            'author_url': 'https://www.youtube.com/channel/UCyn-K7rZLXjGl7VXGweIlcA',
            'height': 270,
            'html': '<iframe width="480" height="270" '
                    'src="https://www.youtube.com/embed/R6IT_f0XPT8?feature=oembed" '
                    'frameborder="0" allow="accelerometer; autoplay; '
                    'encrypted-media; gyroscope; picture-in-picture" '
                    'allowfullscreen></iframe>',
            'provider_name': 'YouTube',
            'provider_url': 'https://www.youtube.com/',
            'thumbnail_height': 360,
            'thumbnail_url': 'https://i.ytimg.com/vi/R6IT_f0XPT8/hqdefault.jpg',
            'thumbnail_width': 480,
            'title': '[백종원의 백종원 레시피] 강식당2 화제의 메뉴! 김치밥이 피오씁니다',
            'type': 'video',
            'version': '1.0',
            'width': 480},
 'status_code': 200,
 'title': '[백종원의 백종원 레시피] 강식당2 화제의 메뉴! 김치밥이 피오씁니다',
 'url': 'https://www.youtube.com/watch?v=R6IT_f0XPT8',
 'videos': [{'height': 270,
             'src': 'https://www.youtube.com/embed/R6IT_f0XPT8?feature=oembed',
             'width': 480}]}
```

위와 같이 이미지나 비디오같은 각종 정보들이 출력됩니다.

```python
sample = lassie.fetch('https://www.youtube.com/watch?v=R6IT_f0XPT8', all_images=True)
pprint(sample)
```

lassie로 웹 컨텐츠를 가져와서 pprint로 출력하면 보기 좋게 출력할 때에 all_images와 같이 다양한 옵션으로 값을 다르게 가져올 수 있습니다.

```python
{'images': [{'height': 360,
             'src': 'https://i.ytimg.com/vi/R6IT_f0XPT8/hqdefault.jpg',
             'width': 480},
            {'src': 'https://s.ytimg.com/yts/img/favicon-vfl8qSV2F.ico',
             'type': 'favicon'},
            {'src': 'https://s.ytimg.com/yts/img/favicon_32-vflOogEID.png',
             'type': 'favicon'},
            {'src': 'https://s.ytimg.com/yts/img/favicon_48-vflVjB_Qk.png',
             'type': 'favicon'},
            {'src': 'https://s.ytimg.com/yts/img/favicon_96-vflW9Ec0w.png',
             'type': 'favicon'},
            {'src': 'https://s.ytimg.com/yts/img/favicon_144-vfliLAfaB.png',
             'type': 'favicon'},
            {'alt': '',
             'src': 'https://www.youtube.com/watch?v=R6IT_f0XPT8',
             'type': 'body_image'},
            {'alt': '',
             'src': 'https://www.youtube.com/watch?v=R6IT_f0XPT8',
             'type': 'body_image'},
            {'alt': '',
             'src': 'https://www.youtube.com/watch?v=R6IT_f0XPT8',
             'type': 'body_image'},
            {'alt': '',
             'src': 'https://www.youtube.com/watch?v=R6IT_f0XPT8',
             'type': 'body_image'},
            {'alt': '',
             'src': 'https://www.youtube.com/watch?v=R6IT_f0XPT8',
             'type': 'body_image'},
            {'alt': '[[emojiAlt(item)]]',
             'height': 24,
             'src': 'https://www.youtube.com/watch?v=R6IT_f0XPT8',
             'type': 'body_image',
             'width': 24},
            {'alt': '',
             'src': 'https://www.youtube.com/watch?v=R6IT_f0XPT8',
             'type': 'body_image'},
            {'alt': '[[altLabelForImage]]',
             'src': 'https://www.youtube.com/watch?v=R6IT_f0XPT8',
             'type': 'body_image'},
            {'alt': '',
             'src': 'https://www.youtube.com/watch?v=R6IT_f0XPT8',
             'type': 'body_image'},
            {'alt': '[[getSimpleText(instruction.previewInstruction.previewHeader)]]',
             'src': 'https://www.youtube.com/watch?v=R6IT_f0XPT8',
             'type': 'body_image'},
            {'alt': '[[getSimpleText(instruction.previewInstruction.fullImageHeader)]]',
             'src': 'https://www.youtube.com/watch?v=R6IT_f0XPT8',
             'type': 'body_image'},
            {'alt': '',
             'src': 'https://www.youtube.com/watch?v=R6IT_f0XPT8',
             'type': 'body_image'},
            {'alt': '[[data.alt]]',
             'src': 'https://www.youtube.com/watch?v=R6IT_f0XPT8',
             'type': 'body_image'},
            {'alt': '',
             'src': 'https://www.youtube.com/watch?v=R6IT_f0XPT8',
             'type': 'body_image'},
            {'alt': '',
             'height': 1,
             'src': 'https://www.youtube.com/watch?v=R6IT_f0XPT8',
             'type': 'body_image',
             'width': 1},
            {'alt': '',
             'height': 1,
             'src': 'https://www.youtube.com/watch?v=R6IT_f0XPT8',
             'type': 'body_image',
             'width': 1},
            {'alt': '',
             'src': 'https://www.youtube.com/watch?v=R6IT_f0XPT8',
             'type': 'body_image'},
            {'alt': '',
             'src': 'https://www.gstatic.com/youtube/img/creator/merch/labs_header_cropped.png',
             'type': 'body_image'},
            {'alt': '',
             'src': 'https://www.youtube.com/watch?v=R6IT_f0XPT8',
             'type': 'body_image'},
            {'alt': '',
             'src': 'https://www.youtube.com/watch?v=R6IT_f0XPT8',
             'type': 'body_image'},
            {'alt': '',
             'src': 'https://www.gstatic.com/youtube/img/branding/youtubeicon/svg/youtube_main_icon.svg',
             'type': 'body_image'}],
 'locale': 'ko_KR',
 'oembed': {'author_name': '백종원의 요리비책',
            'author_url': 'https://www.youtube.com/channel/UCyn-K7rZLXjGl7VXGweIlcA',
            'height': 270,
            'html': '<iframe width="480" height="270" '
                    'src="https://www.youtube.com/embed/R6IT_f0XPT8?feature=oembed" '
                    'frameborder="0" allow="accelerometer; autoplay; '
                    'encrypted-media; gyroscope; picture-in-picture" '
                    'allowfullscreen></iframe>',
            'provider_name': 'YouTube',
            'provider_url': 'https://www.youtube.com/',
            'thumbnail_height': 360,
            'thumbnail_url': 'https://i.ytimg.com/vi/R6IT_f0XPT8/hqdefault.jpg',
            'thumbnail_width': 480,
            'title': '[백종원의 백종원 레시피] 강식당2 화제의 메뉴! 김치밥이 피오씁니다',
            'type': 'video',
            'version': '1.0',
            'width': 480},
 'status_code': 200,
 'title': '[백종원의 백종원 레시피] 강식당2 화제의 메뉴! 김치밥이 피오씁니다',
 'url': 'https://www.youtube.com/watch?v=R6IT_f0XPT8',
 'videos': [{'height': 270,
             'src': 'https://www.youtube.com/embed/R6IT_f0XPT8?feature=oembed',
             'width': 480}]}
```

all_images를 참으로 변경하면 페비콘, 비디오 썸네일뿐만 아니라 해당 웹 페이지의 모든 이미지를 가져오게 됩니다.

```python
l = Lassie()
sample = l.fetch('https://www.youtube.com/watch?v=R6IT_f0XPT8')

pprint(sample)
```

Lassie 객체를 만들어서 웹 컨텐츠를 가져올 수도 있습니다.

```python
l.request_opts = {
    'headers': {
        'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_5) AppleWebKit/605.1.15 (KHTML, like Gecko) '
                      'Version/12.1.1 Safari/605.1.15 '
    }
}

l.request_opts = {
    'timeout': 0.1
}

sample = l.fetch('https://www.youtube.com/watch?v=R6IT_f0XPT8')

pprint(sample)
```

request_opts로 헤더나 타임아웃을 설정할 수 있으며, 만약 타임아웃이 걸린다면  lassie.exceptions.LassieError가 발생됩니다.