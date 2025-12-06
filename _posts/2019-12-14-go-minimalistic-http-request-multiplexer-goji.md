---
layout: post
title: Golang HTTP multiplexer Goji 라이브러리 알아보기
---

오늘은 Golang으로 HTTP 요청을 보낼 수 있는 multiplexer Goji 라이브러리를 알아보려 합니다.

## Goji 설치

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

go get으로 Goji 패키지를 설치합니다.

```
go get goji.io
```

홈의 go 폴더에 Goji 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"net/http"

	"goji.io"
	"goji.io/pat"
)
```

fmt, net 그리고 goji를 가져옵니다.

```go
func handler(w http.ResponseWriter, r *http.Request) {
	parameter := pat.Param(r, "q")
	fmt.Fprintf(w, parameter)
}
```

파라미터를 받아서 ResponseWriter에 작성합니다.

```go
func main() {
	mux := goji.NewMux()
```

mux를 생성합니다.

```go
	mux.HandleFunc(pat.Get("/search/:q"), handler)
```

Get 요청으로 /search 경로에서 파라미터를 받을 수 있게 핸들러를 사용합니다.

```go
	http.ListenAndServe("localhost:8080", mux)
}
```

8080 포트로 서버를 구동합니다.