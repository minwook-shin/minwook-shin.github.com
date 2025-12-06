---
layout: post
title: Golang 2D 이미지 랜더링 gg 라이브러리 알아보기
---

오늘은 Golang으로 2D 이미지 랜더링하는 gg 라이브러리를 알아보려 합니다.

## gg 설치

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

go get으로 gg 패키지를 설치합니다.

```
go get -u github.com/fogleman/gg
```

홈의 go 폴더에 gg 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"github.com/fogleman/gg"
)
```

gg를 가져옵니다.

```go
func main() {
	ggContext := gg.NewContext(1024, 1024)
```

인자로 넣은 넓이와 높이로 image.RGBA를 생성하고 컨텍스트도 준비합니다.

```go
	ggContext.DrawCircle(500, 500, 400)
	ggContext.SetRGB(0, 0, 0)
	ggContext.Fill()
```

원을 그리고 색을 지정하여 칠해줍니다.

```go
	ggContext.SavePNG("circle.png")
```

png 파일로 저장합니다.

```go
	ggContext = gg.NewContext(1024, 1024)
```

인자로 넣은 넓이와 높이로 image.RGBA를 생성하고 컨텍스트도 준비합니다.

```go
	ggContext.SetRGB(1, 1, 1)
	ggContext.Clear()
```

흰색바탕의 배경 색을 만듭니다.

```go
	ggContext.SetRGB(0, 0, 0)
	if err := ggContext.LoadFontFace("/Library/Fonts/Arial.ttf", 100); err != nil {
		panic(err)
	}
	ggContext.DrawStringAnchored("Hello, world!", 1024/2, 1024/2, 1, 1)
```

Arial 폰트의 검은 문자열을 기록합니다.

인자에 넣은 x, y, ax, ay에 맞는 위치에 Hello, world!라고 그려집니다.

```go
	ggContext.SavePNG("text.png")
}
```

png 파일로 저장합니다.