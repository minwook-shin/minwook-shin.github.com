---
layout: post
title: Golang RESTful JSON API Go-Json-Rest 라이브러리 알아보기 2
---

오늘도 Golang으로 RESTful JSON API를 만들 수 있는 Go-Json-Rest 라이브러리를 알아보려 합니다.

## Go-Json-Rest 설치

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

go get으로 Go-Json-Rest 패키지를 설치합니다.

```
go get github.com/ant0ine/go-json-rest/rest
```

홈의 go 폴더에 Go-Json-Rest 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"log"
	"net/http"

	"github.com/ant0ine/go-json-rest/rest"
)
```

log, net 그리고 go-json-rest를 가져옵니다.

```go
func main() {
	restAPI2 := rest.NewApi()
```

처음과 같이 API 객체를 만듭니다.

```go
	restAPI2.Use(rest.DefaultDevStack...)
```

다시 개발에 필요한 로그, 타임존, 오류 처리와 같은 미들웨어를 사용합니다.

```go
	restApp, err := rest.MakeRouter(
		rest.Get("/param/#name", func(w rest.ResponseWriter, req *rest.Request) {
			w.WriteJson(map[string]string{"Body": req.PathParam("name")})
		}),
	)
	if err != nil {
		log.Fatal(err)
	}
```

get 요청을 보낼 수 있는 라우터를 만들어서 파라미터를 받을 수 있게 만들 수 있습니다.

```go
	restAPI2.SetApp(restApp)
	http.Handle("/api/", http.StripPrefix("/api", restAPI2.MakeHandler()))
```

HTTP 요청을 처리하는 핸들러를 반환하여 해당 경로로 접근할 때 미리 만든 API 객체의 라우터가 수행될 수 있게 합니다.

```go
	log.Fatal(http.ListenAndServe(":8080", nil))
}
```

8080 포트로 restful api 서버를 만듭니다.