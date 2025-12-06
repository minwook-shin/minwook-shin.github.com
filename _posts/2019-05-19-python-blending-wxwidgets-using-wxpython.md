---
layout: post
title: Python wxWidgets 구현한 wxPython 라이브러리 알아보기
---

오늘은 Python으로 크로스 플랫폼 GUI API인 wxWidgets를 구현한 래퍼 라이브러리인 wxPython을 적용해보려 합니다.

## wxPython 설치

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
pip install -U wxPython
```

pip로 wxPython을 설치합니다.

## hello world

```python
import wx
```

wxPython을 가져옵니다.

```python
app = wx.App()
```

wxPython 앱 객체를 생성합니다.

```python
frm = wx.Frame(None, title="Hello World")
frm.Show()
```

제목이 포함된 윈도우 프레임을 만들고, 보이게 합니다.

```python
app.MainLoop()
```

이벤트 루프를 생성합니다.

## window

```python
import wx
```

wxPython을 가져옵니다.

```python
def print_hello(event):
    wx.MessageBox("Hello world!")

def about_func(event):
    wx.MessageBox("sample text", "About title", wx.OK | wx.ICON_INFORMATION)
```

메시지 박스를 출력하는 함수를 만들어 놓습니다.

```python
class CustomFrame(wx.Frame):
    def __init__(self, *args, **kw):
        super(HelloFrame, self).__init__(*args, **kw)
```

윈도우 프레임을 구성하는 클래스를 만듭니다.

```python
        pnl = wx.Panel(self)
        wx.StaticText(pnl, label="Hello World!")
```

Hello World!라는 글자를 출력하기 위해 패널에 씁니다.

```python
        file = wx.Menu()
        hello_item = file.Append(-1, "&Hello...\tCtrl-H", "hello!")
```

Ctrl+H의 단축키를 지닌 프로그램의 매뉴를 만들 수 있습니다.

```python
        help_menu = wx.Menu()
        about_item = help_menu.Append(wx.ID_ABOUT)
```

또다른 매뉴를 만들어서 프로그램 정보를 보게 할 수 있습니다.

```python
        menu_bar = wx.MenuBar()
        menu_bar.Append(file, "&File")
        menu_bar.Append(help_menu, "&Help")
```

매뉴바를 생성해서 상단에 추가할 수 있습니다.

wx.Menu()로 만든 매뉴가 인자로 필요합니다.

```python
        self.SetMenuBar(menu_bar)
```

윈도우 프레임에 매뉴를 등록합니다.

```python
        self.Bind(wx.EVT_MENU, print_hello, hello_item)
        self.Bind(wx.EVT_MENU, about_func, about_item)
```

이벤트 헨들러와 이벤트를 바인드합니다.

```python
        self.CreateStatusBar()
        self.SetStatusText("wxPython status text")
```

윈도우 프레임 하단에 있는 상태 바를 생성할 수 있습니다.

```python
if __name__ == '__main__':
    app = wx.App()
    frm = CustomFrame(None, title='basic window')
    frm.Show()
    app.MainLoop()
```

메인 함수를 수행한면 앱의 객체를 생성하고, 직접 작성한 윈도우 프레임 클래스의 하위 클래스를 사용하여 출력합니다.

마지막으로 앱의 메인 루프를 생성합니다.
