---
layout: post
title: Golang CORS handler CORS 라이브러리 알아보기
---

오늘은 Golang으로 HTTP 접근 제어 CORS handler CORS 라이브러리를 알아보려 합니다.

## CORS 설치

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

go get으로 CORS 패키지를 설치합니다.

```
go get github.com/rs/cors
```

홈의 go 폴더에 CORS 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"net/http"

	"github.com/rs/cors"
)
```

net과 cors를 가져옵니다.

```go
func main() {
	serveMux := http.NewServeMux()
```

ServeMux를 생성합니다.

```go
	serveMux.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		w.Header().Set("Content-Type", "application/json")
		w.Write([]byte("{\"message\": \"hello, world!\"}"))
	})
```

헤더와 출력할 바이트를 작성하고 원하는 경로의 핸들러 함수를 등록합니다.

```go
	handler := cors.Default().Handler(serveMux)
```

http 요청에 CORS를 적용합니다.

```go
	corHandler := cors.New(cors.Options{
		AllowedOrigins:   []string{"http://127.0.0.1:8080", "http://example:8080"},
		AllowedMethods:   []string{http.MethodGet},
		AllowedHeaders:   []string{"Origin", "Accept", "Content-Type", "X-Requested-With"},
		AllowCredentials: true,
		MaxAge:           0,
		Debug:            true,
	})
```

Cors 핸들러를 만들 때에 Origins, Methods, Headers에 대한 원하는 옵션을 지정할 수 있습니다.

```go
	handler = corHandler.Handler(handler)
```

위 옵션으로 http 요청에 CORS를 적용합니다.

```go
	http.ListenAndServe(":8080", handler)
}
```

만든 핸들러를 기반으로 8080 포트로 서버를 구동합니다.