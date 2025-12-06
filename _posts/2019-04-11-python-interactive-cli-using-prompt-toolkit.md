---
layout: post
title: Python prompt-toolkit 패키지를 사용하여 대화형 cli 프로그램 만들기
---

오늘은 Python에서 대화형 command line interface를 만들어 주는 prompt_toolkit 패키지에 대하여 알아보려 합니다.

## 개요

prompt_toolkit 패키지는 파이썬에서 구현된 GNU 리드라인입니다.

GNU readline은 command line interface에서 줄 편집과 같은 역할을 하는 라이브러리입니다.

## prompt_toolkit 설치

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
pip install prompt_toolkit
```

pip로 prompt_toolkit을 설치합니다.

## 포맷팅

```python
from prompt_toolkit import print_formatted_text

print_formatted_text('Hello world')
```

형식을 가지고 stdout으로 출력해줍니다.

```python
from prompt_toolkit import print_formatted_text, HTML

print_formatted_text(HTML('<b>bold</b>'))
print_formatted_text(HTML('<i>talic</i>'))
print_formatted_text(HTML('<u>underline</u>'))
```

위와 같이 html 태그 색상을 적용할 수 있습니다.

## 입력

```python
from prompt_toolkit import prompt

text = prompt('input: ')
```

프롬프트를 만들어서 입력을 받을 수 있습니다.

```python
from prompt_toolkit.auto_suggest import AutoSuggestFromHistory
from prompt_toolkit import PromptSession
from prompt_toolkit.history import FileHistory

session = PromptSession(history=FileHistory('my_history'))

text1 = session.prompt('session input1: ',)
text2 = session.prompt('session input2: ', auto_suggest=AutoSuggestFromHistory())
```

세션 객체를 만들어서 연속적으로 프롬프트를 띄울 수 있습니다.

입력 히스토리는 유지됩니다.

그리고 FileHistory를 통하여 이전에 입력한 문자열을 저장할 수도 있습니다.

```python
from prompt_toolkit.completion import WordCompleter
from prompt_toolkit import prompt

word_completer = WordCompleter(['hello', 'world', 'python'])
text = prompt('input: ', completer=word_completer, complete_while_typing=True)
```

completer를 설정하여 특정 단어 자동완성을 할 수 있습니다.

complete_while_typing 옵션으로 작성하는 도중에 자동완성을 활성화할 수 있습니다.

```python
from prompt_toolkit.validation import Validator
from prompt_toolkit import prompt


def is_number(txt):
    return txt.isdigit()


validator = Validator.from_callable(
    is_number,
    error_message='non-numeric characters',
    move_cursor_to_end=True)

number = int(prompt('input: ', validator=validator))
```

validator에 함수를 이용하여 부합하지 않으면 error_message를 출력하게 할 수 있습니다.

## 대화창

```python
from prompt_toolkit.shortcuts import message_dialog, input_dialog, yes_no_dialog, button_dialog

message_dialog(
    title='Example dialog window',
    text='Do you want to continue?\nPress ENTER to quit.')
```

확인만 하는 dialog를 생성할 수 있습니다.

```python
result = yes_no_dialog(
    title='Example dialog window',
    text='Do you want to continue?')
```

예/아니오 버튼이 존재하는 dialog를 생성할 수 있습니다.

```python
input_dialog(
    title='Example dialog window',
    text='input:')
```

입력을 받는 dialog를 생성할 수 있습니다.

```python
button_dialog(
    title='Example dialog window',
    text='Do you want to continue??',
    buttons=[
        ('Yes', True),
        ('No', False),
    ],
)
```

자신이 원하는 버튼을 만들 수 있는 dialog를 생성할 수 있습니다.

## 진행바

```python
from prompt_toolkit.shortcuts import ProgressBar
import time


with ProgressBar() as pb:
    for i in pb(range(100)):
        time.sleep(.01)
```

ProgressBar를 생성할 수 있습니다.
