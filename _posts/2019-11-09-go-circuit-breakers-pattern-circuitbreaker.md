---
layout: post
title: Golang Circuit Breaker 패턴 circuitbreaker 라이브러리 알아보기
---

오늘은 Golang으로 Circuit Breaker 패턴을 구현하는 circuitbreaker 라이브러리를 알아보려 합니다.

## circuitbreaker 설치

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

go get으로 circuitbreaker 패키지를 설치합니다.

```
go get github.com/rubyist/circuitbreaker
```

홈의 go 폴더에 circuitbreaker 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"time"

	circuit "github.com/rubyist/circuitbreaker"
)
```

fmt, time 그리고 circuitbreaker를 가져옵니다.

```go
func main() {
	circuitBreaker := circuit.NewThresholdBreaker(10)
```

10번 실패하면 중단되는 회로 차단기를 생성합니다.

```go
	BreakerEvent := circuitBreaker.Subscribe()
	go func() {
		for {
			<-BreakerEvent
		}
	}()
```

구독으로 BreakerEvents 채널을 반환하고, 함수를 비동기로 수행하며 대기합니다.

```go
	for {
		if circuitBreaker.Ready() {
			httpClient := circuit.NewHTTPClient(time.Second*10, 10, nil)
```

원하는 로직을 호출할 준비가 되면 Ready() 함수는 true를 반환하므로 HTTP 요청을 보내게 됩니다.

```go
			resp, err := httpClient.Get("http://example.com")
			fmt.Println(resp)
```

특정 사이트 응답을 받아와서 출력합니다.

```go
			if err != nil {
				circuitBreaker.Fail()
				continue
			}
			circuitBreaker.Success()
		}
	}
}
```

실패하거나, 성공하는 것을 기록하여 close, open, half-open 상태를 변환합니다.