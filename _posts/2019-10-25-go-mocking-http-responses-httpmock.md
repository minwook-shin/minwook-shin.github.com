---
layout: post
title: Golang HTTP 응답 mocking httpmock 라이브러리 알아보기
---

오늘은 Golang으로 HTTP 응답을 mocking할 수 있는 httpmock 라이브러리를 알아보려 합니다.

## httpmock 설치

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

go get으로 httpmock 패키지를 설치합니다.

```
go get github.com/jarcoal/httpmock
```

홈의 go 폴더에 httpmock 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package test
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언하지 않고 테스트를 위해 다른 이름을 적습니다.

```go
import (
	"fmt"
	"io/ioutil"
	"net/http"
	"testing"

	"github.com/jarcoal/httpmock"
)
```

fmt, ioutil, http, testing 그리고 httpmock을 가져옵니다.

```go
func TestFunc(t *testing.T) {
	httpmock.Activate()
	defer httpmock.DeactivateAndReset()
```

테스트 함수를 만들고, defer 키워드로 Deactivate()와 Reset() 함수를 부릅니다.

```go
	httpmock.RegisterResponder("GET", "http://example.com/test",
		httpmock.NewStringResponder(200, `[{"hello": "world"}]`))

	httpmock.RegisterResponder("GET", `=~^http://example.com/test/id/\d+\z`,
		httpmock.NewStringResponder(200, `{"id": 1, "hello": "world"}`))
```

responder를 등록합니다.

정규표현식으로 인하여 유동적인 responder를 만들 수 있습니다.

```go
	res, _ := http.Get("http://example.com/test")
	body, _ := ioutil.ReadAll(res.Body)
	fmt.Println(string(body))
```

실제 HTTP 요청을 보내는 코드를 실행합니다.

값을 받아보면 위 responder에서 지정한 값대로 출력됩니다.

```go
	res, _ = http.Get("http://example.com/test/id/1")
	body, _ = ioutil.ReadAll(res.Body)
	fmt.Println(string(body))
```

실제 HTTP 요청을 보내는 코드를 실행합니다.

값을 받아보면 위 responder에서 정규표현식과 매치되므로 지정한 값대로 출력됩니다.

```go
	httpmock.GetTotalCallCount()
```

httpmock의 responder가 등록된대로 얼마나 호출되었는지 수를 셉니다.

```go
	countInfo := httpmock.GetCallCountInfo()
	match := countInfo["GET http://example.com/test"]
	fmt.Println(match)

	regexMatch := countInfo[`GET =~^http://example.com/test/id/\d+\z`]
	fmt.Println(regexMatch)
}
```

httpmock의 responder가 등록된대로 얼마나 호출되었는지 각자 확인할 수 있습니다.
