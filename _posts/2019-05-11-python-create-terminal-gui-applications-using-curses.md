---
layout: post
title: Python 터미널 그래픽 애플리케이션 curses 사용하기
---

오늘은 Python 터미널 그래픽 애플리케이션을 만들 수 있는 표준 라이브러리인 curses를 적용해보려 합니다.

## curses 설치

파이썬에 기본적으로 탑제되어 있습니다.

## hello, world

```python
from curses import wrapper
```

curses에서 wrapper를 가져옵니다.

```python
def main(screen):
    screen.clear()
```

화면을 지울 수 있습니다.

```python
    screen.addstr(0, 0, "hello,world!")
```

0번째 줄의 0번째 위치에 문자를 출력할 수 있습니다.

```python
    screen.refresh()
```

화면을 새로 고칠 수 있습니다.

```python
    screen.getkey()
```

키 입력을 받을 때까지 기다립니다.

```python
wrapper(main)
```

curses를 초기화하고, 함수를 불러올 수 있습니다.

## input

```python
import curses
```

curses를 가져옵니다.

```python
def main(screen):
    screen.clear()
    screen.addstr(0, 0, 'Hello World')
```

화면을 지우고, 0번째 줄의 0번째 위치에 문자를 출력할 수 있습니다.

```python
    while True:
        c = screen.getch()
        screen.addstr(1, 0, str(c))
```

문자의 입력을 받아서 1번째 줄의 0번째 위치에 문자를 출력할 수 있습니다.

```python
curses.wrapper(main)
```

curses를 초기화하고, 함수를 불러올 수 있습니다.

## textpad

```python
import curses
from curses.textpad import Textbox, rectangle
```

curses와 curses의 textpad를 가져옵니다.

```python
def main(screen):
    screen.addstr(0, 0, "Enter text")
```

0번째 줄의 0번째에 문자열을 출력할 수 있습니다.

```python
    edit_win = curses.newwin(5, 30, 2, 1)
    rectangle(screen, 1, 0, 1 + 5 + 1, 1 + 30 + 1)
```

새로운 윈도우를 만들고, 직사각형을 만들 수 있습니다.

```python
    screen.refresh()
    box = Textbox(edit_win)
    box.edit()
```

화면을 새로 고치고, 새로 만든 윈도우에 텍스트박스 객체를 반환할 수 있습니다.

```python
    message = box.gather()
    return message
```

윈도우의 작성된 내용을 문자열로 가져옵니다.

```python
msg = curses.wrapper(main)
```

curses를 초기화하고, 함수를 불러올 수 있습니다.
