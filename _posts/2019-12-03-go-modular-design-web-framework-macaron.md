---
layout: post
title: Golang 모듈 디자인 웹 프레임워크 Macaron 라이브러리 알아보기
---

오늘은 Golang으로 만들 수 있는 모듈 디자인 웹 프레임워크 Macaron 라이브러리를 알아보려 합니다.

## Macaron 설치

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

go get으로 Macaron 패키지를 설치합니다.

```
go get gopkg.in/macaron.v1
```

홈의 go 폴더에 Macaron 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"net/http"

	"gopkg.in/macaron.v1"
)
```

net 그리고 macaron를 가져옵니다.

```go
func handler(ctx *macaron.Context) string {
	return "hello, world!"
}
```

핸들러 함수를 만들어서 특정 경로 URL에 접근하여 "hello, world!"로 출력하도록 합니다.

```go
func main() {
	classicMacaron := macaron.Classic()
```

기본적으로 Logger, Recovery 그리고 Static 미들웨어를 사용하여 Macaron 객체를 만듭니다.

```go
	classicMacaron.Get("/", func() string {
		return "root"
	})
```

get 요청을 보내 문자열을 반환합니다.

```go
	classicMacaron.Get("/handler", handler)
```

또는 만들어둔 핸들러 함수를 사용합니다.

```go
	// classicMacaron.Run()
```

기본적으로 Run으로 호출하면 4000 포트로 http 서버가 구동됩니다.

```go
	err := http.ListenAndServe("0.0.0.0:4000", classicMacaron)
	if err != nil {
		panic(err)
	}
}
```

ListenAndServe로 원하는 주소와 포트로 http 서버가 구동됩니다.