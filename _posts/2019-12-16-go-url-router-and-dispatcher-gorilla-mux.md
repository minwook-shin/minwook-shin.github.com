---
layout: post
title: Golang Router와 Dispatcher gorilla/mux 라이브러리 알아보기
---

오늘은 Golang으로 Router와 Dispatcher를 구현하는 gorilla/mux 라이브러리를 알아보려 합니다.

## gorilla/mux 설치

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

go get으로 gorilla/mux 패키지를 설치합니다.

```
go get -u github.com/gorilla/mux
```

홈의 go 폴더에 gorilla/mux 소스코드와 패키지 파일이 생성됩니다.

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

	"github.com/gorilla/mux"
)
```

fmt, log, net 그리고 gorilla의 mux를 가져옵니다.

```go
func logging(next http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		next.ServeHTTP(w, r)
	})
}
```

MiddlewareFunc 함수를 만듭니다.

```go
func handler(w http.ResponseWriter, r *http.Request) {
	w.Write([]byte("hello, world!\n"))
}
```

hello, world!를 웹에 출력하는 핸들러 함수를 구현합니다.

```go
func paramHandler(w http.ResponseWriter, r *http.Request) {
	variables := mux.Vars(r)
	w.WriteHeader(http.StatusOK)
	fmt.Fprintf(w, variables["id"])
}
```

파라미터로 값을 받아서 출력하는 핸들러 함수를 구현합니다.

```go
func main() {
	router := mux.NewRouter()
```

router 객체를 생성합니다.

```go
	router.HandleFunc("/", handler)
	router.HandleFunc("/user/{id}", paramHandler)
	router.HandleFunc("/regex/{id:[0-9]+}", paramHandler)
```

router 객체에 URL 패턴에 부합하는 새로운 라우터를 등록합니다.

```go
	router.Use(logging)
```

미리 만든 MiddlewareFunc 함수를 등록합니다.

```go
	err := http.ListenAndServe(":8080", router)
	if err != nil {
		log.Fatal(err)
	}
}
```

8080 포트로 서버를 구동합니다.