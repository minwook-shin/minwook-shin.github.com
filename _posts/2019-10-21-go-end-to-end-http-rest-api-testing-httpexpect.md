---
layout: post
title: Golang HTTP와 REST API 테스트 프레임워크 httpexpect 라이브러리 알아보기
---

오늘은 Golang으로 HTTP와 REST API를 테스트할 수 있는 테스트 프레임워크 httpexpect 라이브러리를 알아보려 합니다.

## httpexpect 설치

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

go get으로 httpexpect 패키지를 설치합니다.

```
go get github.com/gavv/httpexpect
```

홈의 go 폴더에 httpexpect 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package test
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언하지 않고 테스트를 위해 다른 이름을 적습니다.

```go
import (
	"encoding/json"
	"net/http"
	"net/http/httptest"
	"testing"

	"github.com/gavv/httpexpect"
)
```

json, http, testing 그리고 httpexpect를 가져옵니다.

```go
type userMap map[string]interface{}
```

map 인터페이스를 선언합니다.

```go
func handlerFunc() http.Handler {
	mux := http.NewServeMux()
```

http 핸들러를 반환하는 함수를 만들면서 ServeMux를 생성합니다.

```go
	user := userMap{}
	mux.HandleFunc("/user", func(w http.ResponseWriter, r *http.Request) {
		switch r.Method {
		case "GET":
			v := []string{}
			for u := range user {
				v = append(v, u)
			}
			byteData, err := json.Marshal(v)
			if err != nil {
				panic(err)
			}

			w.Header().Set("Content-Type", "application/json")
			w.Write(byteData)
```

GET, POST, PUT중에 GET 요청을 할 수 있는 /user에 대한 핸들러를 등록합니다.

```go
		default:
			w.WriteHeader(http.StatusMethodNotAllowed)
		}
	})
	return mux
}
```

만약 그 외의 방식으로 접근하면 StatusMethodNotAllowed를 헤더로 전송합니다.

```go
func TestFunc(t *testing.T) {
	handler := handlerFunc()
```

이제 위 핸들러로 만든 서버를 테스트할 테스트 함수를 작성합니다.

```go
	server := httptest.NewServer(handler)
	defer server.Close()
```

httptest로 서버를 생성합니다.

```go
	expect := httpexpect.New(t, server.URL)
```

httptest로 만든 서버를 기반으로 Expect 객체를 생성합니다.

```go
	expect.GET("/user").
		Expect().
		Status(http.StatusOK).JSON().Array().Empty()

	expect.GET("/user/test_name").
		Expect().
		Status(http.StatusNotFound)
}
```

GET 요청을 보낼 때에 반환되는 응답을 테스트합니다.