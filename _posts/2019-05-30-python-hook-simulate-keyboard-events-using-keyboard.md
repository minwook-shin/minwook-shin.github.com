---
layout: post
title: Python 키보드 이벤트를 후킹하고 시뮬레이팅하는 keyboard 라이브러리 알아보기
---

오늘은 Python으로 사용자의 키보드의 입력 이벤트를 후킹하고, 시뮬레이팅할 수 있는 라이브러리인 keyboard를 적용해보려 합니다.

## keyboard 설치

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
pip install keyboard
```

pip로 keyboard를 설치합니다.

## write

```python
import keyboard
import fileinput
```

keyboard와 fileinput을 가져옵니다.

```python
for line in fileinput.input():
	keyboard.write(line)
```

운영체제에 가상 키보드 이벤트를 보내어 키보드 입력을 진행할 수 있습니다.

## macro

```python
import keyboard
import time
```

keyboard와 time을 가져옵니다.

```python
keyboard.start_recording()
time.sleep(10)
events = keyboard.stop_recording()
keyboard.replay(events)
```

잠시 멈추면서 계속 키보드 입력을 기록하고, 그동안 기록한 것을 리플레이할 수 있습니다.

## simulate

```python
import keyboard
import time
```

keyboard와 time을 가져옵니다.

```python
for i in range(10):
    keyboard.press('a')
    time.sleep(0.1)

keyboard.release('a')
```

키보드에 특정 키를 입력하는 이벤트를 시뮬레이팅시킬 수 있습니다.

## press key

```python
import keyboard
```

keyboard를 가져옵니다.

```python
def print_pressed_keys(e):
    for code in keyboard._pressed_events:
        line = ', '.join(str(code))
        print(line,e.name)


keyboard.hook(print_pressed_keys)
keyboard.wait('esc')
```

키보드를 누를 때마다 특정 함수를 호출하는 hook 함수를 사용할 수 있습니다.

마지막으로 esc를 누를 때까지 기다립니다.

## hot key

```python
import keyboard
```

keyboard를 가져옵니다.

```python
shortcut = keyboard.read_hotkey()

def on_triggered():
    print("Triggered!")

keyboard.add_hotkey(shortcut, on_triggered)
keyboard.wait('esc')
```

핫키를 설정하고, add_hotkey 함수로 핫키가 입력될 때마다 특정 함수를 수행할 수 있습니다.

마지막으로 esc를 누를 때까지 기다립니다.
