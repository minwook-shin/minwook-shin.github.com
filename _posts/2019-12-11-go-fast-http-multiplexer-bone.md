---
layout: post
title: Golang HTTP 멀티플렉서 bone 라이브러리 알아보기
---

오늘은 Golang으로 라우터 중심으로 알아보는 HTTP 멀티플렉서 bone 라이브러리를 알아보려 합니다.

## bone 설치

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

go get으로 bone 패키지를 설치합니다.

```
go get github.com/go-zoo/bone
```

홈의 go 폴더에 bone 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"net/http"
	"strconv"

	"github.com/go-zoo/bone"
)
```

net, strconv 그리고 bone을 가져옵니다.

```go
func handler(rw http.ResponseWriter, req *http.Request) {
	num := bone.GetValue(req, "num")
	text := bone.GetValue(req, "text")

	rw.Write([]byte("hello, world! " + num + text))
}
```

handler 함수를 구현합니다.

GetValue로 http.Request의 키 값을 가져와서 출력합니다.

```go
func main() {
	mux := bone.New()
```

Mux 객체를 생성합니다.

```go
	mux.RegisterValidatorFunc("isNum", func(s string) bool {
		if _, err := strconv.Atoi(s); err == nil {
			return true
		}
		return false
	})
```

isNum이라는 Validator를 만듭니다.

```go
	mux.Get("/:num|isNum", http.HandlerFunc(handler))
```

만든 라우터 파라미터 Validator로 Get 메소드를 사용하여 가져오는 값에 대해 숫자인지 검사합니다.

```go
	mux.Get("/:num/:text", http.HandlerFunc(handler))
```

multiple 파라미터로 Get 메소드를 사용하여 값을 가져옵니다.

```go
	mux.Get("/#num^[0-9]$", http.HandlerFunc(handler))
```

정규표현식으로 Get 메소드를 사용하여 가져오는 값에 대해 숫자인지 검사합니다.

```go
	mux.Post("/post", http.HandlerFunc(handler))
```

Post 메소드를 사용할 수도 있습니다.

```go
	mux.Handle("/", http.HandlerFunc(handler))
```

다른 HTTP 메소드 없이 라우터를 추가할 수도 있습니다.

```go
	http.ListenAndServe(":8080", mux)
}
```

8080 포트로 서버를 구동합니다.