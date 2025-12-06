---
layout: post
title: Golang 고수준 API SSH 서버 ssh 라이브러리 알아보기
---

오늘은 Golang으로 이루어진 고수준의 API인 SSH 서버를 구현한 ssh 라이브러리를 알아보려 합니다.

## ssh 설치

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

go get으로 ssh 패키지를 설치합니다.

```
go get github.com/gliderlabs/ssh
```

홈의 go 폴더에 ssh 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"time"

	"github.com/gliderlabs/ssh"
)
```

time과 ssh를 가져옵니다.

```go
func main() {
	MaxTimeout := 20 * time.Second
	IdleTimeout := 10 * time.Second
```

최대 타임아웃 시간과 Idle 상태의 타임아웃을 지정할 수 있습니다.

```go
	ssh.Handle(func(s ssh.Session) {
		i := 0
		for {
			i++
			select {
			case <-time.After(time.Second):
				continue
			case <-s.Context().Done():
				return
			}
		}
	})
```

DefaultHandler로 등록합니다.

채널로 타임아웃이 될 때 세션의 context에서 Done을 보냅니다.

```go
	server := &ssh.Server{
		Addr:        ":2222",
		MaxTimeout:  MaxTimeout,
		IdleTimeout: IdleTimeout,
	}
```

서버 주소와 포트와 이미 만들어둔 time.Duration 변수를 지정합니다.

```go
	err := server.ListenAndServe()
	if err != nil {
		panic(err)
	}
}
```

서버를 구동합니다.