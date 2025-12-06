---
layout: post
title: Python OpenGL과 멀티미디어 사용하는 pyglet 라이브러리 알아보기
---

오늘은 Python으로 대부분 운영체제에서 멀티 미디어를 구동하거나 openGL으로 간단하게 그림을 그릴 수 있는 라이브러리인 pyglet을 적용해보려 합니다.

## pyglet 설치

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
pip install pyglet
```

pip로 pyglet을 설치합니다.

## hello world

```python
import pyglet
```

pyglet을 가져옵니다.

```python
window = pyglet.window.Window()
```

Window 객체를 반환합니다.

```python
label = pyglet.text.Label('Hello, world',
                          font_size=36,
                          x=window.width // 2,
                          y=window.height // 2,
                          anchor_x='center',
                          anchor_y='center')
```

Hello, world라는 문자열을 라벨로 출력합니다.

```python
@window.event
def on_draw():
    label.draw()
```

만든 라벨을 그려줍니다.

```python
pyglet.app.run()
```

pyglet 이벤트와 윈도우를 수행합니다

## multi media

```python
image = pyglet.image.load('image.png')
image.blit(0, 0)
```

```python
music = pyglet.resource.media('music.mp3')
music.play()
```

사진이나 음악을 가져와서 보여주거나 재생할 수 있습니다.

## input

```python
import pyglet
from pyglet.window import mouse, key
```

마우스와 키보드를 인식하기 위해 pyglet의 mouse, key를 가져옵니다.

```python
window = pyglet.window.Window()
```

Window 객체를 반환합니다.

```python
@window.event
def on_key_press(symbol, modifiers):
    if symbol == key.A:
        print('A')
    elif symbol == key.S:
        print('S')
    elif symbol == key.D:
        print('D')
    elif symbol == key.F:
        print('F')
```

on_key_press 이벤트 함수로 키가 눌렸을 때에 조건문을 만들 수 있습니다.

```python
    elif symbol == key.LEFT:
        print('left')
    elif symbol == key.RIGHT:
        print('right')
    elif symbol == key.UP:
        print('up')
    elif symbol == key.DOWN:
        print('down')
```

방향키도 동일하게 처리할 수 있습니다.

```python
@window.event
def on_mouse_press(x, y, button, modifiers):
    if button == mouse.LEFT:
        print('mouse left')
    elif button == mouse.RIGHT:
        print('mouse right')
```

마우스도 왼쪽 클릭과 오른쪽 클릭을 할 수 있습니다.

```python
@window.event
def on_draw():
    window.clear()
```

on_draw 이벤트 함수로 화면이 그려질 때 할 행동을 지정할 수 있습니다.

```python
pyglet.app.run()
```

pyglet 이벤트와 윈도우를 수행합니다

## OpenGL

```python
from pyglet.gl import *
```

pyglet.gl을 가져옵니다.

```python
win = pyglet.window.Window()
```

Window 객체를 반환합니다.

```python
@win.event
def on_draw():
    glBegin(GL_LINES)
    glVertex3f(100.0, 100.0, 0)
    glVertex3f(400.0, 100.0, 0)
    glEnd()

    glBegin(GL_LINES)
    glVertex3f(100.0, 400.0, 0)
    glVertex3f(100.0, 100.0, 0)
    glEnd()

    glBegin(GL_LINES)
    glVertex3f(400.0, 100.0, 0)
    glVertex3f(400.0, 400.0, 0)
    glEnd()

    glBegin(GL_LINES)
    glVertex3f(100.0, 400.0, 0)
    glVertex3f(400.0, 400.0, 0)
    glEnd()
```

openGL로 선을 그립니다.

glBegin으로 그리기를 시작하고, glEnd로 그리는 것을 종료합니다.

glVertex3f로 각각 x, y, z 좌표를 작성하여 서로 두 선을 이을 수 있습니다.

```python
pyglet.app.run()
```

pyglet 이벤트와 윈도우를 수행합니다
