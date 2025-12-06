---
layout: post
title: Golang 플러그인 주도 HTTP 클라이언트 gentleman 라이브러리 알아보기
---

오늘은 Golang으로 플러그인을 설치해서 사용할 수 있는 HTTP 클라이언트인 gentleman 라이브러리를 알아보려 합니다.

## gentleman 설치

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

go get으로 gentleman 패키지를 설치합니다.

```
go get -u gopkg.in/h2non/gentleman.v2
```

홈의 go 폴더에 gentleman 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"gopkg.in/h2non/gentleman.v2"
	"gopkg.in/h2non/gentleman.v2/plugins/compression"
)
```

fmt, gentleman을 가져옵니다.

```go
func main() {
	client := gentleman.New()
	client.URL("http://httpbin.org")
```

gentleman 클라이언트객체를 만들고, 해당 주소로 요청을 보냅니다.

```go
	client.Use(compression.Disable())
```

gentleman은 플러그인을 사용할 수 있습니다.

해당 코드는 compression 플러그인을 사용한 예시입니다.

```go
	request := client.Request()
	request.Path("/headers")
	request.SetHeader("Custom-Header", "Custom-VALUE")
```

현재 클라이언트 요청에 url의 경로를 지정하고, 헤더도 설정할 수 있습니다.

```go
	response, err := request.Send()
	if err != nil {
		panic(err)
	}
	if !response.Ok {
		fmt.Println(response.StatusCode)
		return
	}
	fmt.Println(response.String())
}
```

요청을 보내서 응답으로 상태 코드와 문자열을 반환받을 수 있습니다.