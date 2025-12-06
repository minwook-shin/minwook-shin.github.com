---
layout: post
title: Golang structured leveled 로그 zap 라이브러리 알아보기
---

오늘은 Golang으로 structured leveled한 로그를 남기는 우버의 zap 라이브러리를 알아보려 합니다.

## zap 설치

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

go get으로 zap 패키지를 설치합니다.

```
go get -u go.uber.org/zap
```

홈의 go 폴더에 zap 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"time"

	"go.uber.org/zap"
)
```

time과 zap을 가져옵니다.

```go
func main() {
	logger, _ := zap.NewProduction()
```

Info 레벨으로 기본이 되는 printf 스타일을 지원하는 로거를 생성합니다.

```go
	defer logger.Sync()
```

defer 키워드로 인하여 종료될 때에 로그에 쌓인 버퍼를 지워줍니다.

```go
	sugar := logger.Sugar()
	sugar.Debugw("Debugw",
		"attempt", 3,
		"time", time.Second,
	)
	sugar.Debugf("Debugf")

	sugar.Infow("Infow",
		"attempt", 3,
		"time", time.Second,
	)
	sugar.Infof("Infof")

	sugar.Warnw("Warnw",
		"attempt", 3,
		"time", time.Second,
	)
	sugar.Warnf("Warnf")

	sugar.Errorw("Errorw",
		"attempt", 3,
		"time", time.Second,
	)
	sugar.Errorf("Errorf")
```

로거를 랩핑한 sugar로 로그를 생성할 수 있습니다.

Debug, Info, Warn, Error와 같이 레벨을 나눌 수 있습니다.

```go
	sugar.Fatalw("Fatalw",
		"attempt", 3,
		"backoff", time.Second,
	)
	sugar.Fatalf("Fatalf")

	sugar.Panicw("Panicw",
		"attempt", 3,
		"backoff", time.Second,
	)
	sugar.Panicf("Panicf")
```

이 때에 Fatal, Panic은 exit 1으로 종료되거나 Panic을 일으킵니다.

```go
	logger.Debug("Debug",
		zap.String("strKey", "String"),
		zap.Int("intKey", 1),
		zap.Duration("Duration", time.Second),
	)

	logger.Info("Info",
		zap.String("strKey", "String"),
		zap.Duration("Duration", time.Second),
	)

	logger.Warn("Warn",
		zap.String("strKey", "String"),
		zap.Duration("Duration", time.Second),
	)

	logger.Error("Error",
		zap.String("strKey", "String"),
		zap.Duration("Duration", time.Second),
	)
```

랩핑하지 않고, 로그 메시지를 남길 수도 있습니다.

printf 스타일을 지원하지 않지만 여전히 Debug, Info, Warn, Error와 같이 레벨을 나눌 수 있습니다.

```go
	logger.Fatal("Fatal",
		zap.String("strKey", "String"),
		zap.Duration("Duration", time.Second),
	)

	logger.Panic("Panic",
		zap.String("strKey", "String"),
		zap.Duration("Duration", time.Second),
	)

}
```

이 때도 Fatal, Panic은 exit 1으로 종료되거나 Panic을 일으킵니다.
