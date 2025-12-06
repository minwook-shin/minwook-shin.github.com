---
layout: post
title: Python 텍스트 기반 그래픽 인터페이스 Urwid 라이브러리 알아보기
---

오늘은 Python으로 대부분 운영체제에서 텍스트 기반의 그래픽 유저 인터페이스 라이브러리인 Urwid를 적용해보려 합니다.

## Urwid 설치

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
pip install urwid
```

pip로 urwid를 설치합니다.

## hello world

```python
import urwid
```

urwid를 가져옵니다.

```python
txt = urwid.Text("Hello World")
```

바이트나 유니코드로 이루어진 텍스트를 생성합니다.

```python
fill = urwid.Filler(txt, 'top')
```

Filler 객체를 생성합니다.

```python
loop = urwid.MainLoop(fill)
loop.run()
```

대화형 형식을 유지하기 위한 루프를 만들고 실행합니다.

## input

```python
import urwid
```

urwid를 가져옵니다.

```python
def exit_program(key):
    if key in ('q', 'Q'):
        raise urwid.ExitMainLoop()
    txt.set_text(repr(key))
```

q나 Q를 누르면 메인 루프를 탈출하는 함수를 작성합니다.

만약 다른 문자(또는 마우스) 입력이 들어오면 텍스트로 출력합니다.

```python
txt = urwid.Text("")
fill = urwid.Filler(txt, 'top')
```

텍스트와 함께 Filler 객체를 생성합니다.

```python
loop = urwid.MainLoop(fill, unhandled_input=exit_program)
loop.run()
```

대화형 형식을 유지하기 위한 루프를 만들고 실행합니다.

## menu

```python
import urwid
```

urwid를 가져옵니다.

```python
choices = ["first", "second", "third"]
```

선택할 수 있는 매뉴를 리스트로 선언합니다.

```python
def menu(title, choices):
    body = [urwid.Text(title), urwid.Divider()]
    for c in choices:
        button = urwid.Button(c)
        urwid.connect_signal(button, 'click', exit_program)
        body.append(urwid.AttrMap(button, None, focus_map='reversed'))
    return urwid.ListBox(urwid.SimpleFocusListWalker(body))
```

매뉴를 생성하기 위해 함수를 만듭니다.

버튼을 생성하고, 버튼을 누를 때에 프로그램의 메인 루프가 종료되는 함수를 수행하게 하고 리스트 박스를 반환합니다.

```python
def exit_program(button):
    raise urwid.ExitMainLoop()
```

메인 루프를 나갈 수 있는 함수입니다.

```python
main = urwid.Padding(menu("title", choices))
top = urwid.Overlay(main, urwid.SolidFill(u'\N{MEDIUM SHADE}'),
                    align='center', width=('relative', 10),
                    valign='middle', height=('relative', 50))
```

Padding을 추가하고, 그 위젯 위에 별도의 위젯을 렌더링할 수 있게 Overlay 객체를 사용합니다.

```python
urwid.MainLoop(top).run()
```

대화형 형식을 유지하기 위한 루프를 만들고 실행합니다.
