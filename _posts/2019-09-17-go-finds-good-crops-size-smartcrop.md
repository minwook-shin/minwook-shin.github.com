---
layout: post
title: Golang 자동 사진 자르는 smartcrop 라이브러리 알아보기
---

오늘은 Golang으로 이미지의 크기를 자동으로 잘라주는 smartcrop 라이브러리를 알아보려 합니다.

## smartcrop 설치

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

go get으로 smartcrop 패키지를 설치합니다.

```
go get github.com/muesli/smartcrop
```

홈의 go 폴더에 smartcrop 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"image"
	"image/png"
	"os"

	"github.com/muesli/smartcrop"
	"github.com/muesli/smartcrop/nfnt"
)
```

image, os 그리고 smartcrop을 가져옵니다.

```go
func main() {
	file, _ := os.Open("original.png")
	decodeImage, _, _ := image.Decode(file)
```

자를 이미지 png 파일을 가져와서 디코드합니다.

```go
	analyzer := smartcrop.NewAnalyzer(nfnt.NewDefaultResizer())
	crop, _ := analyzer.FindBestCrop(decodeImage, 250, 250)
```

Resizer로 Analyzer를 반환하고, 해당 Analyzer로 주어진 너비에 가능한 가장 좋은 자를 크기를 반환합니다.

```go
	file, err := os.Create("output.png")
	if err != nil {
		panic(err)
	}
```

저장할 png 파일을 엽니다.

```go
	type SubImager interface {
		SubImage(r image.Rectangle) image.Image
	}
	subImage := decodeImage.(SubImager).SubImage(crop)
	png.Encode(file, subImage)
}
```

디코드된 파일을 Analyzer에 의한 값을 접목하여 크롭하고, 저장할 png 파일에 인코드합니다.