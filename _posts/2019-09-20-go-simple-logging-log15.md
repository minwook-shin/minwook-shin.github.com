---
layout: post
title: Golang 간단 로깅 log15 라이브러리 알아보기
---

오늘은 Golang으로 간단하게 로그를 남길 수 있는 log15 라이브러리를 알아보려 합니다.

## log15 설치

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

go get으로 log15 패키지를 설치합니다.

```
go get github.com/inconshreveable/log15
```

홈의 go 폴더에 log15 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go

import (
	"os"

	log "github.com/inconshreveable/log15"
)
```

os와 log15를 가져옵니다.

```go
func main() {
	logger := log.New("context", "key/value")
```

로거를 생성합니다.

```go
	logger.Info("Info msg", "first", 10, "second", 20, "third", 30)
	logger.Debug("Debug msg", "first", 10, "second", 20, "third", 30)
	logger.Warn("Warn msg", "first", 10, "second", 20, "third", 30)
	logger.Error("Error msg", "first", 10, "second", 20, "third", 30)
	logger.Crit("Crit msg", "first", 10, "second", 20, "third", 30)
```

Info, Debug, Warn, Error, Crit의 레벨이 존재하며, 각 레벨마다 출력되는 색상이 다릅니다.

Info는 초록색, Debug 파란색, Warn은 노란색, Error는 빨간색, Crit은 보라색이며 각 로그에 메시지를 남길 수 있습니다.

INFO[09-19|21:03:43] Info msg                                 context=key/value first=10 second=20 third=30 라고 출력됩니다.

```go
	logger.SetHandler(log.MultiHandler(
		log.StreamHandler(os.Stderr, log.LogfmtFormat()),
		log.LvlFilterHandler(
			log.LvlError,
			log.Must.FileHandler("log.json", log.JsonFormat()))))

	logger.Info("Info msg", "first", 10, "second", 20, "third", 30)
	logger.Debug("Debug msg", "first", 10, "second", 20, "third", 30)
	logger.Warn("Warn msg", "first", 10, "second", 20, "third", 30)
	logger.Error("Error msg", "first", 10, "second", 20, "third", 30)
	logger.Crit("Crit msg", "first", 10, "second", 20, "third", 30)

}
```

로거를 기록하는 방식을 바꾸기 위해 핸들러를 지정할 수 있습니다.

로그 형식을 바꾸거나, 특정 레벨 이상의 로그만 파일로 기록하게 할 수 있습니다.