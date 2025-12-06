---
layout: post
title: Golang 파일 및 MIME filetype 라이브러리 알아보기
---

오늘은 Golang으로 파일 시그니쳐로 파일, MIME 타입을 유추하는 filetype 라이브러리를 알아보려 합니다.

## filetype 설치

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

go get으로 filetype 패키지를 설치합니다.

```
go get github.com/h2non/filetype
```

홈의 go 폴더에 filetype 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"io/ioutil"
	"os"

	"github.com/h2non/filetype"
)
```

fmt, io, os 그리고 filetype 가져옵니다.

```go
func main() {
	bufferByte, _ := ioutil.ReadFile("original.png")
```

png 파일을 바이트로 반환합니다.

```go
	fileType, _ := filetype.Match(bufferByte)
	if fileType == filetype.Unknown {
		panic(filetype.Unknown)
	}

	fmt.Println(fileType.Extension, fileType.MIME.Value)
```

바이트로 파일 시그니쳐를 확인하고, 파일 타입을 유추합니다.

```go
	if filetype.IsImage(bufferByte) {
		fmt.Println("image")
	}
```

바이트로 이미지인지 확인합니다.

```go
	if filetype.IsSupported("png") {
		fmt.Println("supported")
	}
```

파일 확장자가 지원되는 지 확인합니다.

```go
	if filetype.IsMIMESupported("image/png") {
		fmt.Println("MIME supported")
	}
```

MIME 타입이 지원되는 지 확인합니다.

```go
	file, _ := os.Open("original.png")

	header := make([]byte, 8)
	file.Read(header)
	fmt.Println(header)

	if filetype.IsImage(header) {
		fmt.Println("image")
	}
}
```

파일을 열고 헤더를 가져와서 PNG 파일 시그니쳐로 이미지 타입을 유추합니다.