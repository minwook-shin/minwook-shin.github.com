---
layout: post
title: Golang 다목적 HTTP mocking gock 라이브러리 알아보기
---

오늘은 Golang으로 다목적의 HTTP를 mocking할 수 있는 gock 라이브러리를 알아보려 합니다.

## gock 설치

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

go get으로 gock 패키지를 설치합니다.

```
go get -u gopkg.in/h2non/gock.v1
```

홈의 go 폴더에 gock 소스코드와 패키지 파일이 생성됩니다.

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

	"gopkg.in/h2non/gock.v1"
)
```

fmt, ioutil, http, testing 그리고 gock을 가져옵니다.

```go
func TestFunc(t *testing.T) {
	defer gock.Off()
```

테스트 함수를 만든 다음 defer 키워드로 기본 HTTP 인터셉터를 비활성화하고, 모든 mock을 제거합니다.

```go
	gock.New("http://example.com").
		Get("/test").
		Reply(200).
		JSON(map[string]string{"hello": "world"})
```

Get 요청 보낼 HTTP mock을 만듭니다.

http://example.com/test 에서 상태 코드 200과 {"hello": "world"} 문자열을 반환합니다.

```go
	res, _ := http.Get("http://example.com/test")
	fmt.Println(res.StatusCode)

	body, _ := ioutil.ReadAll(res.Body)
	fmt.Println(string(body))
```

실제 코드처럼 요청을 보내면 Status 코드와 본문이 반환됩니다.

```go
	fmt.Println(gock.IsDone())
}
```

mock이 성공적으로 수행되었다면 참으로 출력됩니다.