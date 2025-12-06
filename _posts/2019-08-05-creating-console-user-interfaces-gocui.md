---
layout: post
title: Golang 터미널 인터페이스 만드는 gocui 라이브러리 알아보기
---

오늘은 Golang으로 터미널에서 UI를 만들 수 있는 gocui 라이브러리를 알아보려 합니다.

## gocui 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 github에서 호스팅되고 있는 gocui 패키지를 설치합니다.

```
go get github.com/jroimartin/gocui
```

홈의 go 폴더에 gocui 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/jroimartin/gocui"
)
```

fmt와 gocui를 가져옵니다.

```go
func layout(gui *gocui.Gui) error {
	maxX, maxY := gui.Size()
	if view, err := gui.SetView("hello", maxX/2-5, maxY/2, maxX/2+2, maxY/2+2); err != nil {
		fmt.Fprintln(view, "Hello!")
	}
	return nil
}
```

layout 함수를 터미널 크기를 알아내서 새로운 hello 뷰로 작성합니다.

이름이 없거나, x0 인자가 x1 인자보다 크거나, y0 인자가 y1 인자보다 크면 오류가 발생합니다.

```go
func quit(g *gocui.Gui, v *gocui.View) error {
	return gocui.ErrQuit
}
```

터미널로 빠져나오는 함수도 작성합니다.

```go
func main() {
	gui, err := gocui.NewGui(gocui.OutputNormal)
	if err != nil {
		panic(err)
	}
	defer gui.Close()
```

메인 함수로 Gui 객체를 생성하고, defer 키워드로 종료되기 전에 gui 객체를 닫습니다.

```go
	gui.SetManagerFunc(layout)
```

방금 전에 만든 layout 함수를 manager 함수로 지정합니다.

```go
	if err := gui.SetKeybinding("", gocui.KeyCtrlC, gocui.ModNone, quit); err != nil {
		panic(err)
	}
```

키보드의 ctrl 키와 c키를 같이 눌러서 터미널로 나오도록 키를 바인딩합니다.

```go
	if err := gui.MainLoop(); err != nil && err != gocui.ErrQuit {
		panic(err)
	}
}
```

gocui.ErrQuit가 반환될 때까지 메인 루프를 수행합니다.