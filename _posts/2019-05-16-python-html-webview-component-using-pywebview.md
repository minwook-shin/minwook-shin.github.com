---
layout: post
title: Python 웹뷰를 사용할 수 있는 pywebview 라이브러리 알아보기
---

오늘은 Python으로 대부분 운영체제에서 웹뷰로 웹 사이트를 출력할 수 있는 라이브러리인 pywebview를 적용해보려 합니다.

## pywebview 설치

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
pip install pywebview
```

pip로 pywebview를 설치합니다.

## hello world

```python
import webview
```

webview를 가져옵니다.

```python
webview.create_window('Hello world',
                      'https://www.google.com')
```

기본적으로 창의 이름과 출력할 웹 사이트의 주소를 넣으면 웹뷰가 작동합니다.

## Full-screen

```python
import webview
import threading
```

webview와 threading을 가져옵니다.

```python
def toggle_fullscreen():
    webview.toggle_fullscreen()


if __name__ == '__main__':
    t = threading.Thread(target=toggle_fullscreen)
    t.start()

    webview.create_window("Full-screen window",
                          "https://www.google.com")
```

create_window 함수가 실행되고 난 뒤에 코드 블럭이 수행되므로, 추가적으로 필요한 작업은 대부분 별도의 쓰레드에서 수행해야 합니다.

위 코드와 같은 경우에는 창이 생성된 뒤에 전체 화면으로 전환하기 위해 쓰레드를 이용했습니다.

## multi window

```python
import webview
import threading
```

webview와 threading을 가져옵니다.

```python
def create_window():
    webview.create_window('Window #2',
                          "https://www.google.com")


if __name__ == '__main__':
    t = threading.Thread(target=create_window)
    t.start()
    webview.create_window('Window #1',
                          "https://www.google.com")
```

create_window으로 생성한 뒤에도 새로운 창을 생성하는 것도 쓰레드로 생성해야 합니다.

## Load HTML

```python
import webview
import threading
```

webview와 threading을 가져옵니다.

```python
def load_html():
    webview.load_html('<h1>loaded HTML</h1>')


if __name__ == '__main__':
    t = threading.Thread(target=load_html)
    t.start()

    webview.create_window('Load HTML')
```

url외에도 HTML 코드로 창을 띄울 수 있습니다.

## debug

```python
import webview
```

webview를 가져옵니다.

```python
if __name__ == '__main__':
    webview.create_window('Debug window',
                          'https://www.google.com',
                          debug=True)
```

디버그를 true로 바꾸고 창에 오른쪽 마우스를 누르면, 태그들의 소스코드를 디버그를 할 수 있는 매뉴가 생성됩니다.

## confirm quit

```python
import webview
```

webview를 가져옵니다.

```python
if __name__ == '__main__':
    webview.create_window("Confirm Quit Example",
                          "https://www.google.com",
                          confirm_quit=True)
```

창을 닫을 때에 확인할 수 있는 창을 띄울 수 있습니다.
