---
layout: post
title: Python css를 파싱하거나 생성하는 cssutils 라이브러리 알아보기
---

오늘은 Python으로 HTML css를 파싱하거나 만들어주는 라이브러리인 cssutils를 적용해보려 합니다.

## cssutils 설치

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
pip install cssutils
```

pip로 cssutils를 설치합니다.

## 예제

```python
import cssutils
```

cssutils를 가져옵니다.

```python
sheet = cssutils.parseString('body { color: red }')
print("cssText1: ", sheet.cssText)
```

css를 문자열로 파싱할 수 있습니다.

```python
sheet = cssutils.parseString('body { color: red }')
cssutils.ser.prefs.indent = 4 * ' '
cssutils.ser.prefs.lineNumbers = True
print("cssText2: ", sheet.cssText)
```

라인 수, 탭 간격을 설정할 수 있습니다.

```python
from cssutils import css
```

cssutils에서 css를 가져옵니다.

```python
sheet = css.CSSStyleSheet()
sheet.cssText = u'@import url(example.css) tv;'
print(sheet.cssText)
```

css 스타일시트를 위해 CSSStyleSheet 객체를 사용할 수 있습니다.

```python
style = css.CSSStyleDeclaration()
style['color'] = 'red'
style_rule = css.CSSStyleRule(selectorText=u'body', style=style)
sheet.add(style_rule)
print(sheet.cssText)
```

CSSStyleDeclaration 객체로 단일 CSS 선언 블록을 나타내서 현재 블록에 설정된 스타일 속성을 직접 설정할 수 있습니다.

```python
print(sheet.cssText)
sheet.cssRules[0].media.appendMedium('print')
print(sheet.cssText)
```

newMedium를 추가할 수 있습니다.

```python
sheet.cssRules[1].selectorList.appendSelector('a')
print(sheet.cssText)
```

Selector를 추가할 수 있습니다.
