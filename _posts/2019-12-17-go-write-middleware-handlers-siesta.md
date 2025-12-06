---
layout: post
title: Golang HTTP 핸들러 Siesta 라이브러리 알아보기
---

오늘은 Golang으로 Service 타입의 HTTP 핸들러 Siesta 라이브러리를 알아보려 합니다.

## Siesta 설치

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

go get으로 Siesta 패키지를 설치합니다.

```
go get github.com/VividCortex/siesta
```

홈의 go 폴더에 Siesta 소스코드와 패키지 파일이 생성됩니다.

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
	"time"

	"github.com/VividCortex/siesta"
)
```

fmt, log, net, time 그리고 siesta를 가져옵니다.

```go
func main() {
	service := siesta.NewService("/")
```

서비스 객체를 생성합니다.

```go
	get := "GET"
	service.Route(get, "/", "'Hello, world!'",
		func(w http.ResponseWriter, r *http.Request) {
			fmt.Fprintln(w, "Hello, world!")
		})
```

서비스로 새로운 라우터를 추가하고 Hello, world!를 출력합니다.

```go
	timeHandler := func(c siesta.Context, w http.ResponseWriter, r *http.Request) {
		timeDelta := time.Now().Sub(c.Get("time").(time.Time))
		fmt.Fprintf(w, "%v", timeDelta)
	}
```

contextHandler에서 저장되는 특정 값을 불러와서 출력하는 핸들러를 구현합니다.

```go
	contextHandler := siesta.Compose(func(c siesta.Context, w http.ResponseWriter, r *http.Request) {
		c.Set("time", time.Now())
	}, timeHandler)
```

이미 구현한 핸들러 함수와 값을 저장하는 핸들러 함수를 단일 contextHandler 함수로 구성합니다.

```go
	service.Route("GET", "/time", "usage", contextHandler)
```

만든 contextHandler를 서비스로 새로운 라우터에 추가합니다.

```go
	err := http.ListenAndServe(":8080", service)
	if err != nil {
		log.Fatal(err)
	}
}
```

8080 포트로 서버를 구동합니다.