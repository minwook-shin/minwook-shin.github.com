---
layout: post
title: Golang 이미지 패턴 생성 geopattern 라이브러리 알아보기
---

오늘은 Golang으로 무작위 이미지 패턴을 생성하여 문자열로 출력해주는 geopattern 라이브러리를 알아보려 합니다.

## geopattern 설치

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

go get으로 geopattern 패키지를 설치합니다.

```
go get github.com/pravj/geopattern
```

홈의 go 폴더에 geopattern 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/pravj/geopattern"
)
```

fmt와 geopattern을 가져옵니다.

```go
func main() {
	args := map[string]string{}
	patternFile := geopattern.Generate(args)
	fmt.Println(patternFile)
```

기존적으로 패턴 이미지를 SVG 문자열로 출력합니다.

```go
	args = map[string]string{"color": "#e2b"}
	patternFile = geopattern.Generate(args)
	fmt.Println(patternFile)

	args = map[string]string{"baseColor": "#f9b"}
	patternFile = geopattern.Generate(args)
	fmt.Println(patternFile)

	args = map[string]string{"generator": "diamonds"}
	patternFile = geopattern.Generate(args)
	fmt.Println(patternFile)

	args = map[string]string{"phrase": "O"}
	patternFile = geopattern.Generate(args)
	fmt.Println(patternFile)
```

색상이나 배경색, 모양등을 설정하고 패턴 이미지를 SVG 문자열로 출력합니다.

```go
	args = map[string]string{}
	patternFile = geopattern.Base64String(args)
	fmt.Println(patternFile)
```

패턴 이미지를 Base64 인코딩된 문자열로 출력합니다.

```go
	args = map[string]string{}
	patternFile = geopattern.URIimage(args)
	fmt.Println(patternFile)
}
```

패턴 이미지를 uri로 출력합니다.