---
layout: post
title: Golang 종단간 HTTP API 테스트 baloo 라이브러리 알아보기
---

오늘은 Golang으로 구현된 종단간 HTTP API 테스트할 수 있는 baloo 라이브러리를 알아보려 합니다.

## baloo 설치

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

go get으로 baloo 패키지를 설치합니다.

```
go get -u gopkg.in/h2non/baloo.v3
```

홈의 go 폴더에 baloo 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package test
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언하지 않고 테스트를 위해 다른 이름을 적습니다.

```go
import (
	"errors"
	"net/http"
	"testing"

	"gopkg.in/h2non/baloo.v3"
)
```

errors, http, testing과 baloo를 가져옵니다.

```go
var balooClient = baloo.New("http://httpbin.org")
```

HTTP 요청을 테스트할 baloo 클라이언트 객체를 만듭니다.

```go
const schema = `{
	"title": "Example Schema",
	"type": "object",
	"properties": {
	  "origin": {
		"type": "string"
	  }
	},
	"required": ["origin"]
  }`
```

요청 본문과 같은 스키마를 정의합니다.

```go
func assert(res *http.Response, req *http.Request) error {
	if res.StatusCode >= 400 {
		return errors.New("Invalid response")
	}
	return nil
}
```

assertion 함수를 작성합니다.

StatusCode가 400이상이면 새로 정의한 오류로 작동합니다.

```go
func TestBFunc(t *testing.T) {
	balooClient.Get("/get").
		Expect(t).
		Status(200).
		Header("Server", "nginx").
		Type("json").
		JSONSchema(schema).
		AssertFunc(assert).
		Done()
}
```

테스트 함수를 만들고, 응답 코드, 헤더, 타입, 스키마등을 테스트합니다.