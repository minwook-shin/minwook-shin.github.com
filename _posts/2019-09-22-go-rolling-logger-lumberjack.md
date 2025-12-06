---
layout: post
title: Golang 롤링 로거 Lumberjack 라이브러리 알아보기
---

오늘은 Golang으로 파일 로테이션을 돌릴 수 있는 로거 Lumberjack 라이브러리를 알아보려 합니다.

## Lumberjack 설치

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

go get으로 Lumberjack 패키지를 설치합니다.

```
go get gopkg.in/natefinch/lumberjack.v2
```

홈의 go 폴더에 Lumberjack 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"log"
	"os"
	"os/signal"
	"syscall"

	"gopkg.in/natefinch/lumberjack.v2"
)
```

log, os, syscall 그리고 lumberjack을 가져옵니다.

```go
func main() {
	logStruct := &lumberjack.Logger{
		Filename:   "test_file.log",
		MaxSize:    1,
		MaxBackups: 3,
		MaxAge:     10,
		Compress:   true,
	}
	log.SetOutput(logStruct)
```

lumberjack을 SetOutput로 넣어줍니다.

이때에 로그파일 이름, 크기, 유효일 등을 설정할 수 있습니다.

```go
	c := make(chan os.Signal, 1)
	signal.Notify(c, syscall.SIGHUP)

	go func() {
		for {
			<-c
			logStruct.Rotate()
		}
	}()
```

다음 파일로 Rotate가 돌기 위해서는 채널로 기존 로그 파일을 닫고, 새로운 로그 파일을 만듭니다.

```go
	logStruct.Write([]byte("TEST logs"))
}
```

로그에 기록합니다.