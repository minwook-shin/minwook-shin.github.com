---
layout: post
title: Golang request 클론 GRequests 라이브러리 알아보기
---

오늘은 Golang으로 request의 클론 클라이언트인 GRequests 라이브러리를 알아보려 합니다.

## GRequests 설치

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

go get으로 GRequests 패키지를 설치합니다.

```
go get -u github.com/levigross/grequests
```

홈의 go 폴더에 GRequests 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/levigross/grequests"
)
```

fmt와 grequests를 가져옵니다.

```go
func main() {
	response, err := grequests.Get("http://httpbin.org/get", nil)
	if err != nil {
		panic(err)
	}
```

grequests로 응답을 요청합니다.

```go
	fmt.Println(response.String())
```

받아온 문자열을 반환하여 출력합니다.

```go
	requestOptions := &grequests.RequestOptions{
		Params: map[string]string{"q": "value"},
	}
	response, err = grequests.Get("http://httpbin.org/get?q=test", requestOptions)
```

URL에 추가 할 파라미터를 전달할 때 기존 파라미터가 있는 경우, URL을 덮어씁니다.

```go
	fmt.Println(response.String())
```

받아온 문자열을 반환하여 출력합니다.

```go
	if err := response.DownloadToFile("test.txt"); err != nil {
		panic(err)
	}
```

응답받은 내용들을 파일로 저장합니다.

```go
	response.ClearInternalBuffer()
	fmt.Println(response.String())
}
```

내부에서 사용하는 버퍼를 지웁니다.

버퍼를 지우면 응답을 호출해도 내용이 사라집니다.