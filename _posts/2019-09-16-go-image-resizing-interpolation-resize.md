---
layout: post
title: Golang 이미지 리사이징 Resize 라이브러리 알아보기
---

오늘은 Golang으로 이미지의 크기를 재구성할 수 있는 Resize 라이브러리를 알아보려 합니다.

## Resize 설치

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

go get으로 Resize 패키지를 설치합니다.

```
go get github.com/nfnt/resize
```

홈의 go 폴더에 Resize 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"image/png"
	"os"

	"github.com/nfnt/resize"
)
```

png, os 그리고 resize를 가져옵니다.

```go
func main() {
	file, err := os.Open("original.png")
	if err != nil {
		panic(err)
	}
	img, err := png.Decode(file)
	if err != nil {
		panic(err)
	}
	file.Close()
```

png 파일을 열어서 디코드한 다음에 리사이징 작업을 진행합니다.

```go
	var width uint = 128
	var height uint = 128
```

어느 넓이와 높이를 지정할 지에 대하여 미리 변수로 정해둡니다.

```go
	file, err = os.Create("NearestNeighbor.png")
	if err != nil {
		panic(err)
	}
	png.Encode(file, resize.Resize(width, height, img, resize.NearestNeighbor))
```

최근접 이웃 탐색 방식으로 Interpolation 함수를 정해서 이미지 리사이징 작업을 진행합니다.

```go
	file, err = os.Create("Bilinear.png")
	if err != nil {
		panic(err)
	}
	png.Encode(file, resize.Resize(width, height, img, resize.Bilinear))
```

Bilinear로 Interpolation 함수를 정해서 이미지 리사이징 작업을 진행합니다.

```go
	file, err = os.Create("Bicubic.png")
	if err != nil {
		panic(err)
	}
	png.Encode(file, resize.Resize(width, height, img, resize.Bicubic))
```

Bicubic으로 Interpolation 함수를 정해서 이미지 리사이징 작업을 진행합니다.

```go
	file, err = os.Create("MitchellNetravali.png")
	if err != nil {
		panic(err)
	}
	png.Encode(file, resize.Resize(width, height, img, resize.MitchellNetravali))
```

Mitchell Netravali로 Interpolation 함수를 정해서 이미지 리사이징 작업을 진행합니다.

```go
	file, err = os.Create("Lanczos2.png")
	if err != nil {
		panic(err)
	}
	png.Encode(file, resize.Resize(width, height, img, resize.Lanczos2))

	file, err = os.Create("Lanczos3.png")
	if err != nil {
		panic(err)
	}
	png.Encode(file, resize.Resize(width, height, img, resize.Lanczos3))
```

Lanczos로 Interpolation 함수를 정해서 이미지 리사이징 작업을 진행합니다.

```go
	defer file.Close()
}
```

마지막으로 defer 키워드에 의해 끝날 때 파일을 닫습니다.

> 하지만 해당 라이브러리는 작년부터 유지보수가 이루어지지 않아서 개발자가 직접 x/image/draw 혹은 imagick 패키지를 사용하도록 안내하고 있습니다.