---
layout: post
title: Golang rate-limit HTTP Tollbooth 라이브러리 알아보기
---

오늘은 Golang으로 rate-limit HTTP 요청 제한하는 Tollbooth 라이브러리를 알아보려 합니다.

## Tollbooth 설치

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

go get으로 Tollbooth 패키지를 설치합니다.

```
go get github.com/didip/tollbooth
```

홈의 go 폴더에 Tollbooth 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"net/http"
	"time"

	"github.com/didip/tollbooth"
	"github.com/didip/tollbooth/limiter"
)
```

fmt, net, time 그리고 tollbooth를 가져옵니다.

```go
func handler(w http.ResponseWriter, req *http.Request) {
	w.Write([]byte("Hello, World!"))
}
```

출력할 핸들러 함수를 구현합니다.

```go
func main() {
	newLimiter := tollbooth.NewLimiter(1, nil)
	newLimiter = tollbooth.NewLimiter(1, &limiter.ExpirableOptions{DefaultExpirationTTL: time.Hour})
```

리미터를 생성하거나 한 시간 뒤에 만료되는 옵션을 붙여서 리미터를 생성할 수 있습니다.

```go
	newLimiter.SetIPLookups([]string{"RemoteAddr", "X-Forwarded-For", "X-Real-IP"})
	newLimiter.SetMethods([]string{"GET", "POST"})
```

IP 주소 장소나 메소드를 설정합니다.

```go
	newLimiter.SetTokenBucketExpirationTTL(time.Hour)
	newLimiter.SetBasicAuthExpirationTTL(time.Hour)
	newLimiter.SetHeaderEntryExpirationTTL(time.Hour)
```

커스텀된 expiration TTL로 헤더 항목과 기본 인증 사용자를 만료할 수 있습니다.

```go
	newLimiter.SetMessage("limit is reached")
	newLimiter.SetMessageContentType("text/plain; charset=utf-8")
```

http 요청 한도가 걸리면 커스텀된 메시지와 content-type을 받을 수 있게 합니다.

```go
	newLimiter.SetOnLimitReached(func(w http.ResponseWriter, r *http.Request) {
		fmt.Println("Set On Limit Reached")
	})
```

http 요청이 거부되면 터미널에 문자열을 출력하도록 함수를 구성했습니다.

```go
	http.Handle("/", tollbooth.LimitFuncHandler(newLimiter, handler))
```

속도 제한을 수행하는 미들웨어로 핸들러 함수를 등록합니다.

```go
	http.ListenAndServe(":8000", nil)
}
```

8000 포트로 서버를 구동합니다.