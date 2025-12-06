---
layout: post
title: Golang Hystrix 재시도 로직 HTTP 클라이언트 Heimdall 라이브러리 알아보기
---

오늘은 Golang으로 Hystrix와 유사한 회로 차단기로 재시도 로직을 구현한 HTTP 클라이언트 Heimdall 라이브러리를 알아보려 합니다.

## Heimdall 설치

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

go get으로 Heimdall 패키지를 설치합니다.

```
go get -u github.com/gojektech/heimdall
```

홈의 go 폴더에 Heimdall 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"io/ioutil"
	"time"

	"github.com/gojektech/heimdall"
	"github.com/gojektech/heimdall/httpclient"
)
```

fmt, io, time 그리고 heimdall을 가져옵니다.

```go
func main() {
	backoff := 2 * time.Millisecond
	maxJitter := 5 * time.Millisecond
	retrier := heimdall.NewRetrier(heimdall.NewConstantBackoff(backoff, maxJitter))
```

backoff Interval, maximumJitter Interval로 ConstantBackoff 객체를 만듭니다.

```go
	timeout := 100 * time.Millisecond

	client := httpclient.NewClient(
		httpclient.WithHTTPTimeout(timeout),
		httpclient.WithRetrier(retrier),
		httpclient.WithRetryCount(3))
```

타임아웃과 Retrier와 같은 설정을 마치고 http 클라이언트를 생성합니다.

```go
	response, err := client.Get("http://httpbin.org/get", nil)
	if err != nil {
		panic(err)
	}
```

HTTP GET 요청을 보냅니다.

```go
	data, err := ioutil.ReadAll(response.Body)

	fmt.Println(string(data))
}
```

텍스트의 끝까지 읽고 데이터를 반환하여 출력합니다.