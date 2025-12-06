---
layout: post
title: Python asciimatics 패키지를 사용하여 텍스트 UI 만들기
---

오늘은 Python으로 터미널에서 텍스트 UI를 만들 수 있는 asciimatics 패키지에 대하여 알아보려 합니다.

## 개요

Asciimatics는 모든 플랫폼에서 전체 화면 텍스트 UI를 만드는 패키지입니다.

## Asciimatics 설치

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
pip install asciimatics
```

pip로 asciimatics를 설치합니다.

## 출력

```python
from asciimatics.screen import Screen
from time import sleep
```

화면의 기본 상태를 추적해서 그려질 Screen 클래스를 가져옵니다.

잠시 멈춰줄 time 패키지도 가져옵니다.

```python
def main(screen):
    screen.print_at('Hello world!', 0, 0)
    screen.print_at('Hello world!', 0, 1)
```

x축과 y축을 기점으로 텍스트를 터미널에 그립니다.

```python
    screen.refresh()
```

화면을 새로 고쳐서 그렸던 내용을 출력합니다.

```python
    screen.move(0, 2)
    screen.draw(10, 2)
    screen.refresh()
```

그리는 커서를 지정된 x축과 y축에 놓고, 줄을 그립니다.

그리고 화면을 새로 고칩니다.

```python
    sleep(10)
```

sleep으로 화면을 잠시 지연시킵니다.

```python
if __name__ == '__main__':
    Screen.wrapper(main)
```

만들어둔 클래스로 새로운 Screen을 구축합니다.

## 입력

```python
from asciimatics.screen import Screen
```

화면의 기본 상태를 추적해서 그려질 Screen 클래스를 가져옵니다.

```python
def main(screen):
    while True:
        screen.print_at('input : ', 0, 0)
        screen.refresh()
```

x축과 y축을 0으로 기준하여 텍스트를 터미널에 그리고 새로 고칩니다.

```python
        ev = screen.get_key()
        if ev in (ord('Q'), ord('q')):
            return
        if ev is not None:
            screen.print_at(chr(ev), 0, 1)
        screen.refresh()
```

get_key로 키보드 입력을 받고, 만약 Q가 입력되면 while문을 탈출하여 해당 작업을 종료합니다.

그 외의 키가 입력되었을 때에 문자를 출력합니다.

```python
Screen.wrapper(main)
```

만들어둔 클래스로 새로운 Screen을 구축합니다.

## 트리 그리기

```python
from asciimatics.effects import Print
from asciimatics.renderers import StaticRenderer
from asciimatics.scene import Scene
from asciimatics.screen import Screen
```

asciimatics 패키지를 가져옵니다.

```python
tree = r"""
       ${3,1}*
      / \
     /   \
    /_   _\
     /   \
    /     \
   /       \
  /__     __\
    /     \
   /       \
  /         \
 /___________\
      ${3}|||
      ${3}|||
"""
```

트리를 그립니다.

```python
def main(screen):
    effects = [
        Print(screen, StaticRenderer(images=[tree]),
              colour=Screen.COLOUR_GREEN, x=0, y=0)
    ]
    screen.play([Scene(effects, -1)], stop_on_resize=True)
```

지정된 위치에 지정된 텍스트 이미지를 출력할 수 있는 Print와 해당 장면을 재생할 수 있는 play로 터미널에 트리를 그릴 수 있습니다.

```python
while True:
    Screen.wrapper(main)
```

위 코드와 마찬가지로 새로운 Screen을 구축합니다.
