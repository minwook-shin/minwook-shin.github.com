---
layout: post
title: Golang JSON HTML 템플릿 렌더링 Render 라이브러리 알아보기
---

오늘은 Golang으로 JSON과 HTML 템플릿을 렌더링하는 Render 라이브러리를 알아보려 합니다.

## Render 설치

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

go get으로 Render 패키지를 설치합니다.

```
go get gopkg.in/unrolled/render.v1
```

홈의 go 폴더에 Render 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"net/http"

	"gopkg.in/unrolled/render.v1"
)
```

net과 render를 가져옵니다.

```go
func main() {
	renderObject := render.New()
	serveMux := http.NewServeMux()
```

Render와 ServeMux 객체를 만듭니다.

```go
	serveMux.HandleFunc("/", func(w http.ResponseWriter, req *http.Request) {
		w.Write([]byte("hello, world!"))
	})
```

바이트를 출력하는 핸들러 함수를 등록합니다.

```go
	serveMux.HandleFunc("/data", func(w http.ResponseWriter, req *http.Request) {
		renderObject.Data(w, http.StatusOK, []byte("hello, world!"))
	})
```

바이너리 데이터를 출력하는 핸들러 함수를 등록합니다.

```go
	serveMux.HandleFunc("/text", func(w http.ResponseWriter, req *http.Request) {
		renderObject.Text(w, http.StatusOK, "hello, text!")
	})
```

텍스트를 출력하는 핸들러 함수를 등록합니다.

```go
	serveMux.HandleFunc("/json", func(w http.ResponseWriter, req *http.Request) {
		renderObject.JSON(w, http.StatusOK, map[string]string{"hello": "json"})
	})
```

JSON을 출력하는 핸들러 함수를 등록합니다.

```go
	serveMux.HandleFunc("/jsonp", func(w http.ResponseWriter, req *http.Request) {
		renderObject.JSONP(w, http.StatusOK, "callback", map[string]string{"hello": "jsonp"})
	})
```

JSON을 출력하는 핸들러 함수를 등록합니다.

```go
	serveMux.HandleFunc("/html", func(w http.ResponseWriter, req *http.Request) {
		renderObject.HTML(w, http.StatusOK, "test", "text")
	})
```
	
콜백을 포함한 JSON을 출력하는 핸들러 함수를 등록합니다.

```go
	http.ListenAndServe("127.0.0.1:8080", serveMux)
}
```

지정한 주소와 포트로 서버를 구동합니다.