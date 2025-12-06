---
layout: post
title: Golang 이미지 처리 알고리즘 bild 라이브러리 알아보기
---

오늘은 Golang으로 이미지를 처리할 수 있는 알고리즘들을 모아둔 bild 라이브러리를 알아보려 합니다.

## bild 설치

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

go get으로 bild 패키지를 설치합니다.

```
go get github.com/anthonynsimon/bild
```

홈의 go 폴더에 bild 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/anthonynsimon/bild/adjust"
	"github.com/anthonynsimon/bild/blur"
	"github.com/anthonynsimon/bild/effect"
	"github.com/anthonynsimon/bild/imgio"
	"github.com/anthonynsimon/bild/segment"
	"github.com/anthonynsimon/bild/transform"
)
```

fmt와 bild를 가져옵니다.

```go
func main() {
	image, err := imgio.Open("original.png")
	if err != nil {
		fmt.Println(err)
		return
	}
```

original.png라는 그림 파일을 준비하고, image 객체를 만듭니다.

```go
	rotate := transform.Rotate(image, 40, nil)
	if err := imgio.Save("rotate.png", rotate, imgio.PNGEncoder()); err != nil {
		fmt.Println(err)
		return
	}
```

이미지를 회전합니다.

Encoder를 PNGEncoder로 설정하여 이미지 파일로 저장할 수 있습니다.

```go
	brightness := adjust.Brightness(image, 0.20)
	if err := imgio.Save("brightness.png", brightness, imgio.PNGEncoder()); err != nil {
		fmt.Println(err)
		return
	}
```

이미지의 밝기를 조절합니다.

Encoder를 PNGEncoder로 설정하여 이미지 파일로 저장할 수 있습니다.

```go
	contrast := adjust.Contrast(image, -0.5)
	if err := imgio.Save("contrast.png", contrast, imgio.PNGEncoder()); err != nil {
		fmt.Println(err)
		return
	}
```

이미지의 명암을 조절합니다.

Encoder를 PNGEncoder로 설정하여 이미지 파일로 저장할 수 있습니다.

```go
	hue := adjust.Hue(image, -40)
	if err := imgio.Save("hue.png", hue, imgio.PNGEncoder()); err != nil {
		fmt.Println(err)
		return
	}
```

이미지의 색조를 조절합니다.

Encoder를 PNGEncoder로 설정하여 이미지 파일로 저장할 수 있습니다.

```go
	box := blur.Box(image, 8.0)
	if err := imgio.Save("box.png", box, imgio.PNGEncoder()); err != nil {
		fmt.Println(err)
		return
	}
```

희미한 정도를 조절합니다.

Encoder를 PNGEncoder로 설정하여 이미지 파일로 저장할 수 있습니다.

```go
	edgeDetection := effect.EdgeDetection(image, 1.0)
	if err := imgio.Save("edgeDetection.png", edgeDetection, imgio.PNGEncoder()); err != nil {
		fmt.Println(err)
		return
	}
```

가장자리를 강조합니다.

Encoder를 PNGEncoder로 설정하여 이미지 파일로 저장할 수 있습니다.

```go
	emboss := effect.Emboss(image)
	if err := imgio.Save("emboss.png", emboss, imgio.PNGEncoder()); err != nil {
		fmt.Println(err)
		return
	}
```

그림이 그림자로 대체합니다.

Encoder를 PNGEncoder로 설정하여 이미지 파일로 저장할 수 있습니다.

```go
	grayscale := effect.Grayscale(image)
	if err := imgio.Save("grayscale.png", grayscale, imgio.PNGEncoder()); err != nil {
		fmt.Println(err)
		return
	}
```

그레이 스케일로 대체합니다.

Encoder를 PNGEncoder로 설정하여 이미지 파일로 저장할 수 있습니다.

```go
	invert := effect.Invert(image)
	if err := imgio.Save("invert.png", invert, imgio.PNGEncoder()); err != nil {
		fmt.Println(err)
		return
	}
```

이미지를 반전합니다.

Encoder를 PNGEncoder로 설정하여 이미지 파일로 저장할 수 있습니다.

```go
	sepia := effect.Sepia(image)
	if err := imgio.Save("sepia.png", sepia, imgio.PNGEncoder()); err != nil {
		fmt.Println(err)
		return
	}
```

세피아 톤으로 설정합니다.

Encoder를 PNGEncoder로 설정하여 이미지 파일로 저장할 수 있습니다.

```go
	sobel := effect.Sobel(image)
	if err := imgio.Save("sobel.png", sobel, imgio.PNGEncoder()); err != nil {
		fmt.Println(err)
		return
	}
```

Sobel-Feldman 연산자에 근사값으로 가장자리를 강조합니다.

Encoder를 PNGEncoder로 설정하여 이미지 파일로 저장할 수 있습니다.

```go
	threshold := segment.Threshold(image, 120)
	if err := imgio.Save("threshold.png", threshold, imgio.PNGEncoder()); err != nil {
		fmt.Println(err)
		return
	}
}
```

들어오는 값에 따라 검은색이나 흰색으로 설정합니다.

Encoder를 PNGEncoder로 설정하여 이미지 파일로 저장할 수 있습니다.