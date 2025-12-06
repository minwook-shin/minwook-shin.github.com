---
layout: post
title: Golang 시스템 애니메이션 Robotgo 라이브러리 알아보기
---

오늘은 Golang으로 시스템의 애니메이션을 제어할 수 있는 Robotgo 라이브러리를 알아보려 합니다.

## Robotgo 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

맥에서 Golang의 버전을 올리려면 brew를 이용합니다.

```
brew upgrade go
```

우분투에서도 Golang의 버전을 올리려면 backports 저장소를 등록하고 apt를 이용합니다.

```
sudo add-apt-repository ppa:longsleep/golang-backports
sudo apt-get update
sudo apt-get install golang-go
```

go get으로 Robotgo 패키지를 설치합니다.

GCC도 같이 설치되어 있어야 합니다.

```
go get github.com/go-vgo/robotgo
```

홈의 go 폴더에 Robotgo 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/go-vgo/robotgo"
)
```

fmt와 robotgo를 가져옵니다.

```go
func main() {
	robotgo.TypeStr("Hello World")
	robotgo.TypeString("Hello World")
```

문자열을 타이핑할 수 있습니다.

TypeString은 unicode를 지원합니다.

```go
	robotgo.KeyTap("enter")
```

enter 키를 누르게 해줍니다.

```go
	robotgo.WriteAll("WriteAll")
	text, err := robotgo.ReadAll()
	if err == nil {
		fmt.Println(text)
	}
```

WriteAll로 문자열을 클립보드에 복사하고, ReadAll로 출력할 수 있습니다.

```go
	robotgo.ScrollMouse(1, "up")
```

마우스를 스크롤합니다.

```go
	robotgo.MoveMouseSmooth(0, 0)
	robotgo.MouseClick("left", true)
```

마우스를 0, 0 좌표로 움직이고, 왼쪽 클릭을 할 수 있습니다.

```go
	x, y := robotgo.GetMousePos()
	fmt.Println(x, y)

	color := robotgo.GetPixelColor(x, y)
	fmt.Println(color)
```

마우스의 위치를 구하고, 해당 위치의 색상을 가져올 수 있습니다.

```go
	bitmap := robotgo.CaptureScreen(1, 1, 3000, 6000)
	robotgo.SaveBitmap(bitmap, "SaveBitmap.png")
	defer robotgo.FreeBitmap(bitmap)
```

화면을 캡쳐해서 파일로 저장할 수 있습니다.

```go
	event := robotgo.AddEvent("mleft")
	if event {
		fmt.Println("press")
	}
```

왼쪽 클릭을 하기 전에 코드 수행을 기다릴 수 있습니다.

```go
	terminalPid, err := robotgo.FindIds("Terminal")

	isExist, err := robotgo.PidExists(terminalPid[0])
	if err == nil && isExist {
		robotgo.Kill(terminalPid[0])
	}
```

Terminal이라는 프로세스 이름이 존재하면 해당 프로세스를 끌 수 있습니다.

```go
	isAlert := robotgo.ShowAlert("제목", "메시지")
	if isAlert == 0 {
		fmt.Println("ok")
	}
}
```

Alert를 출력할 수 있습니다.