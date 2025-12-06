---
layout: post
title: Golang 재시도 로직 retry 라이브러리 알아보기
---

오늘은 Golang으로 성공할 때까지 반복하는 retry 라이브러리를 알아보려 합니다.

## retry 설치

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

go get으로 retry 패키지를 설치합니다.

```
go get github.com/kamilsk/retry
```

홈의 go 폴더에 retry 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"context"
	"fmt"
	"math/rand"
	"net/http"
	"time"

	"github.com/kamilsk/retry"
	"github.com/kamilsk/retry/backoff"
	"github.com/kamilsk/retry/jitter"
	"github.com/kamilsk/retry/strategy"
)
```

context, fmt, math, net, time 그리고 retry를 가져옵니다.

```go
func main() {
	action := func(uint) (err error) {
		defer func() {
			if r := recover(); r != nil {
				panic(r)
			}
		}()
		request, err := http.NewRequest(http.MethodGet, "http://example.com", nil)
		if err != nil {
			return err
		}
		response, err := http.DefaultClient.Do(request)
		fmt.Println(response)
		return err
	}
```

http의 NewRequest로 특정 주소에 get 방식의 요청을 보내는 액션을 정의합니다.

```go
	strategies := retry.How{
		strategy.Limit(2),
		strategy.BackoffWithJitter(
			backoff.Fibonacci(5*time.Millisecond),
			jitter.NormalDistribution(
				rand.New(rand.NewSource(time.Now().UnixNano())), 0.25),
		),
		func(attempt uint, err error) bool {
			return attempt == 0 || err != nil
		},
	}
```

재시도 횟수를 제한하거나 backoff의 알고리즘과 대기 시간을 정의할 수 있습니다.

```go
	breaker, _ := context.WithTimeout(context.Background(), time.Second)
	if err := retry.Try(breaker, action, strategies...); err != nil {
		panic(err)
	}
	fmt.Println("success")
}
```

Try로 주어진 조건내로 성공할 때까지 반복하여 수행합니다.

통과하면 success가 출력됩니다.