---
layout: post
title: Golang 그래픽 인터페이스 구현하는 Fyne 사용해보기
---

오늘은 깃허브 트랜드를 보던 도중에 알게 되었던, Golang으로 크로스 플랫폼 GUI를 구현할 수 있는 Fyne 라이브러리를 직접 사용해보려 합니다.

## 깃허브 url

https://github.com/fyne-io/fyne

## 개요

OpenGL을 사용하여 크로스 플랫폼으로 GUI를 구현한 Go언어 라이브러리입니다.

OpenGL은 go-gl과 go-glfw으로 이루어져 있다고 합니다.

## 설치

gcc와 같은 c 컴파일은 준비되어야하며, openGL을 사용하기 때문에 데비안 계열의 리눅스에서는 libgl1-mesa-dev와 xorg-dev 패키지가 필요합니다.

그리고 fyne.io/fyne를 설치합니다.

```
go get fyne.io/fyne
```

## hello world

간단하게 gui를 띄워보려 합니다.

```go
package main

import (
	"fyne.io/fyne/app"
	"fyne.io/fyne/widget"
)
```

fyne에서 app과 widget을 가져옵니다.

```go
func main() {
	app := app.New()
	w := app.NewWindow("test project")
```

OpenGL driver를 사용하는 app 객체와 윈도우를 생성합니다.

```go
	w.SetContent(widget.NewVBox(
		widget.NewLabel("Hello golang!"),
		widget.NewButton("Quit", func() {
			app.Quit()
		}),
  ))
```

NewVBox는 인자에 들어오는 오브젝트들을 사용하여 세로 박스 위젯을 만듭니다.

NewLabel과 NewButton으로 텍스트 라벨과 버튼을 만들 수 있습니다.

NewButton에는 버튼을 눌렀을 때에 반응하는 tap 핸들러 함수를 등록해야 합니다.
위 코드에서는 앱을 나가는 Quit()으로 구현했습니다.

```go
	w.ShowAndRun()
}
```

마지막으로 앱을 실행해줍니다.

## dialog

```go
dialog.ShowInformation("타이틀", "information msg...", win)
```

ShowInformation으로 정보를 보여 줄 다이얼로그를 출력합니다.

인자에는 타이틀과 메시지, 그리고 부모 윈도우의 정보를 넣으면 됩니다.

win은 아까 생성한 app.NewWindow()입니다.

```go
err := errors.New("error msg")
dialog.ShowError(err, win)
```

오류를 출력하는 다이얼로그에는 문자열이 아닌 error 객체와 부모 윈도우가 들어갑니다.

```go
confirmDialog := dialog.NewConfirm("확인", "Are you sure you want to exit the program?", (func(res bool) { fmt.Println(res) }), win)
confirmDialog.SetDismissText("no")
confirmDialog.SetConfirmText("Yes")
confirmDialog.Show()
```

사용자의 선택을 받는 다이얼로그에는 타이틀과 메시지, 그리고 사용자의 선택에 따라 bool값이 달리지는 콜백을 구현해야 합니다.

## input

```go
entry := widget.NewEntry()
entry.SetPlaceHolder("Entry")
```

문자열을 입력받는 input을 구현할 수 있습니다.

입력한 문자열은 entry.Text로 받을 수 있습니다.

## button

```go
widget.NewButton("Text", func() { fmt.Println("tapped") }),
widget.NewButtonWithIcon("icon", theme.ConfirmIcon(), func() { fmt.Println("tapped") }),
```

아이콘이 있는 버튼과 없는 버튼을 만들 수 있습니다.

## check

```go
widget.NewCheck("Check", func(on bool) { fmt.Println(on, entry.Text) }),
```

NewCheck으로 체크 버튼을 만들 수 있습니다.

체크 박스의 변동이 일어날 때에 발생하는 핸들러를 등록해야 합니다.

## radio

```go
widget.NewRadio([]string{"1", "2"}, func(s string) { fmt.Println(s) }),
```

NewRadio로 라디오 버튼을 만들 수 있습니다.

라디오 버튼의 옵션을 지정하고, 변동이 일어날 때에 발생하는 핸들러를 등록해야 합니다.

## progress

```go
progress := widget.NewProgressBar()
infProgress := widget.NewProgressBarInfinite()
```

흔히 로딩바라고 불리는 진행 표시줄은 NewProgressBar로 구현할 수 있으며, 무한으로 처리하려면 NewProgressBarInfinite로 구현하면 됩니다.

진행 표시줄의 최소는 0이고, 최대는 1이므로 SetValue(0)하면 0%, SetValue(1)하면 100%입니다.

## full screen

```go
win.SetFullScreen(!win.FullScreen())
```

SetFullScreen으로 전체 화면으로 만들 수 있습니다.
