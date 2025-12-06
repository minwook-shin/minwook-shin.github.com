---
layout: post
title: Golang HTTP 구현체 fasthttp 라이브러리 알아보기
---

오늘은 Golang으로 빠른 HTTP 구현체 fasthttp 라이브러리를 알아보려 합니다.

## fasthttp 설치

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

go get으로 fasthttp 패키지를 설치합니다.

```
go get -u github.com/valyala/fasthttp
```

홈의 go 폴더에 fasthttp 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/valyala/fasthttp"
)
```

fmt와 fasthttp를 가져옵니다.

```go
func main() {
	requestHandler := fasthttp.CompressHandler(requestHandler)
```

헤더에 'gzip' 또는 'deflate' 'Accept-Encoding'가 포함되면 압축합니다.

```go
	if err := fasthttp.ListenAndServe(":8080", requestHandler); err != nil {
		panic(err)
	}
}
```

작성한 핸들러와 원하는 포트로 서버를 구동합니다.

```go
func requestHandler(ctx *fasthttp.RequestCtx) {
	fmt.Fprintf(ctx, "%s", &ctx.Request)
	fmt.Fprintf(ctx, "%s", &ctx.Response)
```

Request와 Response의 내용을 Fprintf로 웹 페이지에 출력합니다.

```go
	ctx.SetContentType("text/plain; charset=utf8")
```

Content-Type을 설정합니다.

```go
	ctx.Response.Header.Set("X-Custom-Header", "Custom-header-value")
```

헤더를 설정합니다.

```go
	var cookie fasthttp.Cookie
	cookie.SetKey("cookie-name")
	cookie.SetValue("cookie-value")
	ctx.Response.Header.SetCookie(&cookie)
}
```

쿠키의 키와 값을 설정해서 포함시킵니다.