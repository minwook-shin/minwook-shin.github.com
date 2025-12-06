---
layout: post
title: Golang 할당하지 않는 json 로그 zerolog 라이브러리 알아보기
---

오늘은 Golang으로 할당하지 않고 빠르게 json 로그를 남길 수 있는 zerolog 라이브러리를 알아보려 합니다.

## zerolog 설치

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

go get으로 zerolog 패키지를 설치합니다.

```
go get -u github.com/rs/zerolog/log
```

홈의 go 폴더에 zerolog 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"os"

	"github.com/rs/zerolog"
	"github.com/rs/zerolog/log"
)
```

os, zerolog를 가져옵니다.

```go
func main() {
	zerolog.TimeFieldFormat = zerolog.TimeFormatUnix
```

UNIX Time으로 맞춰줍니다.

```go
	log.Print("Print")
```

기본적으로 debug 레벨로 설정되어 있는 Print로 로그를 출력하게 할 수 있습니다.

```go
	log.Info().Str("strKey", "strVal").Msg("Msg")
```

컨텍스트로 특정 타입의 변수를 키와 값으로 저장할 수 있습니다.

```go
	zerolog.SetGlobalLevel(zerolog.InfoLevel)
	log.Info().Msg("Info")
```

특정 레벨의 로그만 보이게 전역적으로 설정할 수 있습니다.

```go
	logger := zerolog.New(os.Stderr).With().Timestamp().Logger()
	logger.Info().Str("strKey", "strVal").Msg("Msg")

	logger = log.With().Str("strKey", "strVal").Logger()
	logger.Info().Msg("Msg")
```

Logger()로 인하여 logger 객체를 생성해서 출력할 수 있습니다.

```go
	log.Info().Str("strKey", "strVal").
		Dict("dict", zerolog.Dict().
			Str("strKey", "strVal"),
		).Msg("Msg")
```

dictionary를 포함하여 로그를 출력할 수 있습니다.

```go
	sampled := log.Sample(&zerolog.BasicSampler{N: 10})
	sampled.Info().Msg("Msg")
```

로그를 샘플링할 수 있습니다.

```go
	log.Logger = log.With().Caller().Logger()
	log.Info().Msg("Msg")
```

파일의 로그가 작성되어 있는 라인 넘버를 같이 포함하여 로그를 출력할 수 있습니다.

```go
	zerolog.TimestampFieldName = "TimestampFieldName"
	zerolog.LevelFieldName = "LevelFieldName"
	zerolog.MessageFieldName = "MessageFieldName"
	log.Info().Msg("Msg")
```

필드의 이름을 커스터마이징할 수 있습니다.

```go
	log.Logger = log.Output(zerolog.ConsoleWriter{Out: os.Stderr})
	log.Info().Str("strKey", "strVal").Msg("Msg")
}
```

출력의 모양에 변화를 줄 수 있습니디.