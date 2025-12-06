---
layout: post
title: Golang 이벤트 루프 네트워킹 gnet 라이브러리 알아보기
---

오늘은 Golang으로 이벤트 루프 기반의 네트워킹 gnet 라이브러리를 알아보려 합니다.

## gnet 설치

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

go get으로 gnet 패키지를 설치합니다.

```
go get -u github.com/panjf2000/gnet
```

홈의 go 폴더에 gnet 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"github.com/panjf2000/gnet"
)
```

gnet을 가져옵니다.

```go
type echoServer struct {
	*gnet.EventServer
}
```

EventServer 구조체를 만듭니다.

```go
func (es *echoServer) React(c gnet.Conn) (out []byte, action gnet.Action) {
	top, tail := c.ReadPair()
	out = top
	if tail != nil {
		out = append(top, tail...)
	}
```

서버 데이터를 보낼 때 수행하는 함수를 구현합니다.

해당 함수는 서버가 클라이언트로부터 입력 데이터를 수신하면 호출됩니다.

```go
	c.ResetBuffer()
	return
}
```

ring 버퍼를 초기화합니다.

```go
func main() {
	echo := new(echoServer)
```

echoServer 구조체를 객체로 할당합니다.

```go
	err := gnet.Serve(echo, "tcp://:9000", gnet.WithMulticore(true))
	if err != nil {
		panic(err)
	}
}
```

인자로 건내준 지정된 주소와 포트로 이벤트를 핸들링합니다.