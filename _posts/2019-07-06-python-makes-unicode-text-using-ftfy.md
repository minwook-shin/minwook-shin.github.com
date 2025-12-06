---
layout: post
title: Python 결함 문자열을 고치는 ftfy 라이브러리 알아보기
---

오늘은 Python에서 결함있는 문자열을 유니코드 텍스트로 자동 변환되는 ftfy 라이브러리를 알아보려 합니다.

## ftfy 설치

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
pip install ftfy
```

pip로 ftfy를 설치합니다.

## ftfy 예제

```python
from ftfy import fix_text, explain_unicode
```

ftfy에서 문자열을 고치는 fix_text와 유니코드를 디버깅하는 explain_unicode를 가져옵니다.

```python
print(fix_text('uÌˆnicode'))
print(fix_text('&lt;3'))
print(fix_text("&macr;\\_(ã\x83\x84)_/&macr;"))
len(fix_text(''))

explain_unicode('ノ( º _ ºノ) 테스트')
```

문자열을 고치는 fix_text를 사용할 수 있으며, explain_unicode는 각 문자가 어떤 유니코드인지 확인할 수 있습니다.

## fixes

```python
from ftfy.fixes import fix_encoding, unescape_html, uncurl_quotes, fix_line_breaks, decode_escapes
```

ftfy.fixes를 가져옵니다.

```python
print(fix_encoding('â\x81”.'))

print(unescape_html('&lt;hr&gt;'))

print(uncurl_quotes('\u201ctest\u201d'))

print(fix_line_breaks("1. hello\u2028" "2. world"))

factoid = '\\u20a2'
print(decode_escapes(factoid))
```

인코딩, html 문자열을 고치거나 유닉스 스타일의 개행도 수정할 수 있습니다.

## formatting

```python
from ftfy.formatting import character_width, display_center
```

ftfy.formatting을 가져옵니다.

```python
print(character_width('A'))
print(character_width('가'))
```

문자가 표시될 넓이를 출력합니다.

```python
lines = ['Display center', 'center']
for line in lines:
    print(display_center(line, 20, fillchar = '▒'))
```

문자를 가운데로 정렬하기 위해 fillchar를 양 옆으로 채웁니다. 