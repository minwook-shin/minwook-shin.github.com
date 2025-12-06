---
layout: post
title: Python 마크다운 문법을 파싱하는 Mistune 라이브러리 알아보기
---

오늘은 Python으로 마크다운 문법을 파싱하는 Mistune 패키지를 알아보려 합니다.

## Mistune 설치

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
pip install mistune
```

pip로 Mistune을 설치합니다.

## hello world!

간단하게 기존의 마크다운 문법을 파싱하여 html 코드로 출력할 수 있습니다.

```python
import mistune
```

mistune을 가져옵니다.

```python
markdown = mistune.markdown('python **hello, world**!')
print(markdown)
```

마크다운 형식을 html로 랜더링해주는 markdown을 사용합니다.

```python
renderer = mistune.Renderer(escape=True, hard_wrap=True)
```

기본 html Renderer를 사용할 수 있습니다.

```python
markdown = mistune.Markdown(renderer=renderer)
markdown_text = markdown("python **hello, world**!")
print(markdown_text)
```

마크다운을 html로 랜더링할 때에 기본 html Renderer를 사용하는 예시입니다.

## Renderer, Lexer

정규 표현식으로 새로운 파싱 규칙을 만들어 Renderer에 적용할 수 있습니다.

```python
import re
from mistune import Renderer, InlineLexer, Markdown
```

mistune과 정규표현식을 처리할 re를 가져옵니다.

```python
class LinkRenderer(Renderer):
    def wiki_document_link(self, alt, link):
        return '<a href="%s">%s</a>' % (link, alt)
```

Renderer 클래스를 만들어서 html로 변환할 형식을 반환해줍니다.

```python
class LinkInlineLexer(InlineLexer):
    def enable_link(self):
        self.rules.wiki_document_link = re.compile(r'\[\[([\s\S]+?\|[\s\S]+?)\]\](?!\])')
        self.default_rules.insert(0, 'wiki_document_link')
```

Lexer 클래스를 만들어서 정규 표현식으로 파싱 규칙을 정의할 수 있습니다.

```python
default_rules = [
        'escape', 'inline_html', 'autolink', 'url',
        'footnote', 'link', 'reflink', 'nolink',
        'double_emphasis', 'emphasis', 'code',
        'linebreak', 'strikethrough', 'text',
    ]
```

만약 위와 같은 기본 규칙에서 존재하고 있다면 정규 표현식이 따로 필요없습니다.

```python
    def output_wiki_document_link(self, m):
        text = m.group(1)
        print(text)
        alt, link = text.split('|')
        return self.renderer.wiki_document_link(alt, link)
```

규칙의 이름 앞에 output_이 붙은 함수를 만들어서, 정규 표현식 패턴에 일치하는 문자열로 처음에 정의한 랜더러 함수를 수행합니다.

```python
renderer = LinkRenderer()
inline = LinkInlineLexer(renderer)
```

이제 Renderer, Lexer 클래스가 준비되었으므로 객체를 만들어줍니다.

```python
inline.enable_link()
```

파싱 규칙을 등록합니다.

```python
markdown = Markdown(renderer, inline=inline)
html_text = markdown('[[Link Text|Wiki Link]]')
print(html_text)
```

등록된 파싱 규칙과 함께 마크다운 문자열을 넣으면 정규 표현식을 거치면서 랜더링됩니다.