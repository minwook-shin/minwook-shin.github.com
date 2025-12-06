---
layout: post
title: Golang HTTP 라우터 chi 라이브러리 알아보기
---

오늘은 Golang으로 HTTP 서비스를 만들 때에 라우터를 생성하는 chi 라이브러리를 알아보려 합니다.

## chi 설치

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

go get으로 chi 패키지를 설치합니다.

```
go get -u github.com/go-chi/chi
```

홈의 go 폴더에 chi 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"net/http"
	"time"

	"github.com/go-chi/chi"
	"github.com/go-chi/chi/middleware"
)
```

net, time 그리고 chi를 가져옵니다.

```go
func main() {
	mux := chi.NewRouter()
```

Mux 객체를 만듭니다.

```go
	mux.Use(middleware.RequestID)
	mux.Use(middleware.RealIP)
	mux.Use(middleware.Logger)
	mux.Use(middleware.Recoverer)
```

Mux 미들웨어 스택들을 사용할 수 있습니다.

```go
	mux.Use(middleware.Timeout(60 * time.Second))
```

타임아웃도 설정할 수 있습니다.

```go
	mux.Get("/", func(w http.ResponseWriter, r *http.Request) {
		w.Write([]byte("hello world!"))
	})
```

hello world!를 출력하는 GET 메소드를 추가합니다.

```go
	http.ListenAndServe(":8080", mux)
}
```

8080 포트로 서버를 구동합니다.