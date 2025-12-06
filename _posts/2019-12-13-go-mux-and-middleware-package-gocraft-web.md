---
layout: post
title: Golang Mux, Middleware 구성하는 gocraft/web 라이브러리 알아보기
---

오늘은 Golang으로 Mux와 미들웨어를 구성할 수 있는 gocraft/web 라이브러리를 알아보려 합니다.

## gocraft/web 설치

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

go get으로 gocraft/web 패키지를 설치합니다.

```
go get github.com/gocraft/web
```

홈의 go 폴더에 gocraft/web 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"github.com/gocraft/web"
	"net/http"
	"strings"
)
```

fmt, net, strings 그리고 gocraft/web을 가져옵니다.

```go
type context struct {
	Num int
}
```

구조체를 만듭니다.

```go
func (c *context) testNum(rw web.ResponseWriter, req *web.Request, next web.NextMiddlewareFunc) {
	c.Num = 10
	next(rw, req)
}
```

미들웨어 핸들러를 만듭니다.

```go
func (c *context) print(rw web.ResponseWriter, req *web.Request) {
	fmt.Fprint(rw, strings.Repeat("Hello ", c.Num))
}
```

Hello라는 문자열이 반복되는 핸들러 함수도 만듭니다.

```go
func main() {
	router := web.New(context{}).
		Middleware(web.LoggerMiddleware).
		Middleware(web.ShowErrorsMiddleware).
		Middleware((*context).testNum).
		Get("/", (*context).print)
```

GET 요청으로 처리될 라우터를 생성하고, 미들웨어를 등록합니다.

```go
	http.ListenAndServe("localhost:8080", router)
}

```

8080 포트로 서버를 구동합니다.