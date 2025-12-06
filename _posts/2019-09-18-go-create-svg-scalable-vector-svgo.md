---
layout: post
title: Golang SVG 생성 SVGo 라이브러리 알아보기
---

오늘은 Golang으로 SVG를 생성할 수 있는 SVGo 라이브러리를 알아보려 합니다.

## SVGo 설치

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

go get으로 SVGo 패키지를 설치합니다.

```
go get github.com/ajstarks/svgo
```

홈의 go 폴더에 SVGo 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"net/http"

	svg "github.com/ajstarks/svgo"
)
```

http와 svgo를 가져옵니다.

```go
func handlerFunc(w http.ResponseWriter, req *http.Request) {
	w.Header().Set("Content-Type", "image/svg+xml")
	svg := svg.New(w)
```

핸들러 함수를 만들어서 HTTP로 호출할 때에 svg를 생성하도록 할 수 있습니다.

만약 svg.New에 os.Stdout으로 인자를 준다면 터미널에 코드로 출력됩니다.

```go
	width := 1000
	height := 1000
	svg.Start(width, height)
	svg.Circle(width/2, height/2, 200, "style")
	svg.Text(width/2, height/2, "Hello, World!", "text-anchor:middle;font-size:50px;fill:white")
	svg.End()
}
```

원을 그리고, Hello World라는 문자열을 출력하게 할 수 있습니다.

```go
func main() {
	http.Handle("/test", http.HandlerFunc(handlerFunc))
	err := http.ListenAndServe(":8080", nil)
	if err != nil {
		panic(err)
	}
}
```

http://127.0.0.1:8080/test 주소를 들어가면 핸들러 함수에 의하여 svg가 아래와 같이 생성되고 웹 브라우저에서 그려집니다.

```
<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="1000" height="1000">
<circle cx="500" cy="500" r="200" style="style"/>
<text x="500" y="500" style="text-anchor:middle;font-size:50px;fill:white">Hello, World!</text>
</svg>
```