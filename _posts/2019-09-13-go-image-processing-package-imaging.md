---
layout: post
title: Golang 이미지 처리 패키지 Imaging 라이브러리 알아보기
---

오늘은 Golang으로 이미지 처리를 할 수 있는 방식을 모아둔 Imaging 라이브러리를 알아보려 합니다.

## Imaging 설치

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

go get으로 Imaging 패키지를 설치합니다.

```
go get -u github.com/disintegration/imaging
```

홈의 go 폴더에 Imaging 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"github.com/disintegration/imaging"
)
```

imaging을 가져옵니다.

```go
func main() {
	src, err := imaging.Open("original.png")
	if err != nil {
		panic(err)
	}
```

이미지 소스를 가져옵니다.

```go
	src = imaging.CropAnchor(src, 300, 300, imaging.Center)
```

사각형의 이미지 크기를 만들기 위해 특정 부분을 중심으로 잘라줍니다.

위 코드와 같은 경우에는 imaging.Center로 설정했습니다.

```go
	blur := imaging.Blur(src, 5)
	err = imaging.Save(blur, "blur.jpg")
	if err != nil {
		panic(err)
	}
```

준비된 이미지 소스에 가우시안 함수로 블러 처리를 합니다.

효과가 적용된 소스를 이미지 파일로 저장합니다.

```go
	sharpen := imaging.Sharpen(src, 1.5)
	err = imaging.Save(sharpen, "sharpen.jpg")
	if err != nil {
		panic(err)
	}
```

준비된 이미지 소스에 선명하게 합니다.

효과가 적용된 소스를 이미지 파일로 저장합니다.

```go
	adjustGamma := imaging.AdjustGamma(src, 1.25)
	err = imaging.Save(adjustGamma, "adjustGamma.jpg")
	if err != nil {
		panic(err)
	}
```

준비된 이미지 소스에 감마 보정을 합니다.

효과가 적용된 소스를 이미지 파일로 저장합니다.

```go
	adjustContrast := imaging.AdjustContrast(src, 20)
	err = imaging.Save(adjustContrast, "adjustContrast.jpg")
	if err != nil {
		panic(err)
	}
```

준비된 이미지 소스에 대비를 보정합니다.

효과가 적용된 소스를 이미지 파일로 저장합니다.

```go
	adjustBrightness := imaging.AdjustBrightness(src, 20)
	err = imaging.Save(adjustBrightness, "adjustBrightness.jpg")
	if err != nil {
		panic(err)
	}
```

준비된 이미지 소스에 밝기를 보정합니다.

효과가 적용된 소스를 이미지 파일로 저장합니다.

```go
	adjustSaturation := imaging.AdjustSaturation(src, -30)
	err = imaging.Save(adjustSaturation, "adjustSaturation.jpg")
	if err != nil {
		panic(err)
	}
```

준비된 이미지 소스에 채도를 보정합니다.

효과가 적용된 소스를 이미지 파일로 저장합니다.

```go
	grayscale := imaging.Grayscale(src)
	err = imaging.Save(grayscale, "grayscale.jpg")
	if err != nil {
		panic(err)
	}
```

준비된 이미지 소스에 그레이 스케일을 생성합니다.

효과가 적용된 소스를 이미지 파일로 저장합니다.

```go
	invert := imaging.Invert(src)
	err = imaging.Save(invert, "invert.jpg")
	if err != nil {
		panic(err)
	}
```

준비된 이미지 소스에 반전 효과를 줍니다.

효과가 적용된 소스를 이미지 파일로 저장합니다.

```go
	convolve3x3 := imaging.Convolve3x3(
		src,
		[9]float64{
			-1, 0, 1,
			-1, 0, 1,
			-1, 0, 1,
		},
		nil,
	)
	err = imaging.Save(convolve3x3, "convolve3x3.jpg")
	if err != nil {
		panic(err)
	}
}
```

준비된 이미지 소스에 3x3 크기의 마스크로 공간 필터링을 합니다.

효과가 적용된 소스를 이미지 파일로 저장합니다.