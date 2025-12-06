---
layout: post
title: Golang timed rate limiter go-rate 라이브러리 알아보기
---

오늘은 Golang으로 API의 부하 방지에 사용하는 timed rate limiter go-rate 라이브러리를 알아보려 합니다.

## go-rate 설치

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

go get으로 go-rate 패키지를 설치합니다.

```
go get github.com/beefsack/go-rate
```

홈의 go 폴더에 go-rate 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"time"

	"github.com/beefsack/go-rate"
)
```

fmt, time 그리고 go-rate를 가져옵니다.

```go
var rateLimiter = rate.New(3, time.Second)
```

rate limiter를 생성합니다.

```go
func main() {
	now := time.Now()
	for i := 1; i <= 5; i++ {
		rateLimiter.Wait()
		fmt.Println(i, time.Now().Sub(now))
	}
```

5번 반복해도 rate limiter에 의해 차단되면서 속도가 제어됩니다.

```go
	rateLimiter1 := rate.New(1, time.Second)
	rateLimiter2 := rate.New(2, time.Second*3)

	for i := 1; i <= 5; i++ {
		rateLimiter1.Wait()
		rateLimiter2.Wait()
		fmt.Println(i, time.Now().Sub(now))
	}
```

여러 개의 rate limiter가 있으면 번갈아가며 차단되면서 속도가 제어됩니다.

```go
	for i := 1; i <= 5; i++ {
		if ok, remaining := rateLimiter.Try(); ok {
			fmt.Println(fmt.Sprintln(i))
		} else {
			fmt.Println(remaining)
		}
	}
```

rate limiter에 의해 차단되지 않으면서 속도가 제어됩니다.

```go
	time.Sleep(time.Second / 2)
	if ok, remaining := rateLimiter.Try(); ok {
		fmt.Println("Okay1")
	} else {
		fmt.Println(remaining)
	}
}
```

이미 rate limiter 보다 이르기 때문에 Okay1이 출력되지 않습니다.