---
layout: post
title: Golang 표준 HTTP 핸들러 HttpRouter 라이브러리 알아보기
---

오늘은 Golang으로 라우팅 패턴의 변수를 지원하는 표준 HTTP 핸들러 HttpRouter 라이브러리를 알아보려 합니다.

## HttpRouter 설치

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

go get으로 HttpRouter 패키지를 설치합니다.

```
go get github.com/julienschmidt/httprouter
```

홈의 go 폴더에 HttpRouter 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"log"
	"net/http"

	"github.com/julienschmidt/httprouter"
)
```

fmt, log, net 그리고 httprouter를 가져옵니다.

```go
func indexHandler(w http.ResponseWriter, r *http.Request, _ httprouter.Params) {
	fmt.Fprint(w, "hello, world!\n")
}
```

hello, world를 출력하는 핸들러 함수를 만듭니다.

```go
func paramHandler(w http.ResponseWriter, r *http.Request, ps httprouter.Params) {
	fmt.Fprintf(w, ps.ByName("q"))
}
```

파라미터로 값을 가져와서 출력할 수 있는 핸들러 함수를 만듭니다.

```go
func main() {
	router := httprouter.New()
```

라우터를 생성합니다.

```go
	router.GET("/", indexHandler)
	router.GET("/:q", paramHandler)
```

생성한 라우터 객체로 GET 메소드를 할 수 있는 경로를 만듭니다.

```go
	err := http.ListenAndServe(":8080", router)
	if err != nil {
		log.Fatal(err)
	}
}
```

router 객체를 기반으로 8080포트로 서버를 구동합니다.