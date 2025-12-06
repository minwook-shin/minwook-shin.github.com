---
layout: post
title: Python 키보드, 마우스를 지정한대로 움직이게 하는 PyAutoGUI 라이브러리 알아보기
---

오늘은 Python에서 키보드와 마우스의 행동을 자동으로 움직이게 할 수 있는 PyAutoGUI 라이브러리를 알아보려 합니다.

## PyAutoGUI 설치

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


windows:

```
pip.exe install pyautogui
```

linux:

```
pip3 install python3-xlib
sudo apt-get install scrot
sudo apt-get install python3-tk
sudo apt-get install python3-dev
pip3 install pyautogui
```

mac:

```
pip3 install pyobjc-core
pip3 install pyobjc
pip3 install pyautogui
```

각 운영체제에 맞게 위와 같은 방식으로 PyAutoGUI를 설치합니다.

## hello world 타이핑

```python
import pyautogui
```

pyautogui를 가져옵니다.

```python
pyautogui.FAILSAFE = True
```

오류가 발생해도 계속 진행하게 하고 싶다면 False로 설정합니다.

```python
width, height = pyautogui.size()
pyautogui.moveTo(width / 2, height / 2)
```

화면의 크기를 구해서 절반의 길이로 화면 중앙에 마우스를 놓을 수 있습니다.

```python
pyautogui.click()
pyautogui.doubleClick()
```

마우스의 클릭과 더블 클릭을 할 수 있습니다.

```python
pyautogui.typewrite('print("Hello world!")', interval=0.2)
```

문자를 타이핑할 수 있습니다.

```python
pyautogui.keyDown('shift')
pyautogui.press(['left', 'left', 'left', 'left', 'left', 'left', 'left', 'left'])
pyautogui.keyUp('shift')
```

keyDown으로 키를 계속 누르고 있다가 keyUp으로 해당 키에서 손을 때는 효과를 볼 수 있습니다.

위 코드는 shift 키를 누르고 왼쪽 방향키를 눌러서 드래그한 효과를 내는 예시입니다.

## 마우스 움직이기

```python
import pyautogui
```

pyautogui를 가져옵니다.

```python
pyautogui.FAILSAFE = True
```

오류가 발생해도 계속 진행하게 하고 싶다면 False로 설정합니다.

```python
width, height = pyautogui.size()
pyautogui.moveTo(width / 2, height / 2)
pyautogui.moveTo(500, 500, duration=2, tween=pyautogui.easeInOutBounce)
```

```python
pyautogui.moveTo(width / 2, height / 2)
pyautogui.moveTo(500, 500, duration=2, tween=pyautogui.easeInOutQuad)
```

```python
pyautogui.moveTo(width / 2, height / 2)
pyautogui.moveTo(500, 500, duration=2, tween=pyautogui.easeInOutBack)
```

화면의 크기를 구해서 절반의 길이로 화면 중앙에 마우스를 놓을 수 있습니다.

그런 후에 moveTo로 마우스를 움직일 때에 tween에 의해 효과를 줄 수 있습니다.

## 스크롤

```python
import time
import pyautogui
```

pyautogui와 time을 가져옵니다.

```python
time.sleep(3)

width, height = pyautogui.size()
pyautogui.moveTo(width / 2, height / 2)
```

일정 시간 뒤에 화면 가운데에 마우스를 놓습니다. 

```python
for i in range(10):
    pyautogui.scroll(-10, x=width / 2, y=height / 2)
    time.sleep(0.5)
```

마우스를 가운데에 놓고, 반복적으로 스크롤을 아래로 내릴 수 있습니다.

## 스크린샷

```python
import pyautogui
```

pyautogui를 가져옵니다.

```python
pyautogui.FAILSAFE = True

pyautogui.screenshot('test.png')
```

test.png라는 이름으로 해당 파이썬 파일을 구동하고 있는 상태로 스크린샷을 찍어 저장할 수 있습니다.