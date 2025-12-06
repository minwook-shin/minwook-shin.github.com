---
layout: post
title: Python 멀티 플랫폼을 지원하는 GUI 툴킷 Toga 라이브러리 알아보기
---

오늘은 Python으로 대부분 운영체제에서 GUI를 구현할 수 있는 툴킷 라이브러리인 Toga를 적용해보려 합니다.

## Toga 설치

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
pip install --pre toga
```

pip로 Toga를 설치합니다.

## hello world

버튼을 누르면 hello world가 콘솔로 출력되는 예제입니다.

```python
import toga
```

toga를 가져옵니다.

```python
def handler(widget):
    print("hello")
```

콘솔에 hello가 출력되는 함수를 작성합니다.

```python
def build(app):
    box = toga.Box()
    button = toga.Button('Hello world', on_press= handler)
    box.add(button)
    return box
```

박스와 버튼 객체를 만들고, 버튼이 눌리면 콘솔에 hello가 출력되는 함수를 수행하게 합니다.

해당 박스 객체에 버튼을 추가합니다.

```python
def main():
    return toga.App(name='First App', app_id='io.github.minwook-shin.helloworld', startup=build)
```

앱의 이름과 아이디를 정합니다.

```python
if __name__ == '__main__':
    main().main_loop()
```

응용 프로그램을 호출하는 루프를 실행합니다.

## input

```python
import toga
from toga.style.pack import *
```

toga를 가져옵니다.

```python
def build(app):
    def calculate(widget):
        try:
            result_form.value = int(input_form.value) * int(input_form.value)
        except ValueError:
            result_form.value = 'ValueError'
```

위젯의 값을 불러와서 계산하는 함수를 만듭니다.

```python
    box = toga.Box()

    input_box = toga.Box()
    box.add(input_box)
    input_form = toga.TextInput()
    input_form.style.update(flex=1)
    input_box.add(input_form)
    input_box.style.update(direction=ROW)

    result_box = toga.Box()
    box.add(result_box)
    result_form = toga.TextInput(readonly=True)
    result_box.add(result_form)
    result_box.style.update(direction=ROW)
    result_form.style.update(flex=1)

    button = toga.Button('confirm', on_press=calculate)
    button.style.update(flex=1)
    box.add(button)
    box.style.update(direction=COLUMN)

    return box
```

박스를 만들고 입력을 받는 폼 박스, 받은 입력을 계산해서 보여주기만 하는 폼 박스도 만들어줍니다.

버튼을 누르면 위젯의 값을 불러와서 계산하는 함수가 수행됩니다.

```python
def main():
    return toga.App(name='Converter', app_id='io.github.minwook-shin.Converter', startup=build)
```

앱의 이름과 아이디를 정합니다.

```python
if __name__ == '__main__':
    main().main_loop()
```

응용 프로그램을 호출하는 루프를 실행합니다.
