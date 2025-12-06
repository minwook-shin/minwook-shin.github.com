---
layout: post
title: Golang 구조화 로깅 Logrus 라이브러리 알아보기
---

오늘은 Golang으로 필드를 포함하여 로그를 남길 수 있는 Logrus 라이브러리를 알아보려 합니다.

## Logrus 설치

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

go get으로 Logrus 패키지를 설치합니다.

```
go get github.com/sirupsen/logrus
```

홈의 go 폴더에 Logrus 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"os"

	"github.com/sirupsen/logrus"
)
```

os와 logrus를 가져옵니다.

```go
func main() {
	logrus.SetFormatter(&logrus.JSONFormatter{})
```

해당 라인 다음부터 로그를 출력하면 JSON 포맷으로 출력할 수 있습니다.

```go
	logrus.SetFormatter(&logrus.TextFormatter{})
	logrus.SetFormatter(&logrus.TextFormatter{
		DisableColors: false,
		FullTimestamp: true,
	})
```

JSON 포맷으로 출력하지않고, 텍스트로 출력하면서 각종 옵션을 부여할 수 있습니다.

```go
	logrus.SetReportCaller(false)
```

로그를 가져오는 방식을 로그에 포함할 수 있습니다.

```go
	logrus.SetOutput(os.Stdout)
```

표준 출력으로 로그를 출력할 지, 파일로 저장할지도 정할 수 있습니다.

```go
	logrus.SetLevel(logrus.TraceLevel)
```

특정 레벨의 로그부터 출력할 수 있게 정할 수 있으며, 해당 코드로는 Trace 레벨 로그부터 출력하게 지정합니다.

```go
	logrus.WithFields(logrus.Fields{"Fields": "TEST"}).Trace("Trace")
	logrus.WithFields(logrus.Fields{"Fields": "TEST"}).Debug("Debug")
	logrus.WithFields(logrus.Fields{"Fields": "TEST"}).Info("Info")
	logrus.WithFields(logrus.Fields{"Fields": "TEST"}).Warn("Warn")
	logrus.WithFields(logrus.Fields{"Fields": "TEST"}).Error("Error")
```

로그에 필드를 추가하고 Trace, Debug, Info, Warn, Error 레벨의 로그를 남길 수 있습니다.

```go
	logrus.WithFields(logrus.Fields{"Fields": "TEST"}).Fatal("Fatal")
	logrus.WithFields(logrus.Fields{"Fields": "TEST"}).Panic("Panic")
```

단, Fatal은 exit 1으로 강제 종료되고, Panic은 Panic을 일으킵니다.

```go
	entry := logrus.WithFields(logrus.Fields{"Fields": "TEST"})
	entry.Trace("Trace logger")
	entry.Debug("Debug logger")
	entry.Info("Info logger")
	entry.Warn("Warn logger")
	entry.Error("Error logger")
```

변수로 필드를 미리 포함시켜서 Trace, Debug, Info, Warn, Error 레벨의 로그를 남길 수 있습니다.

```go
	entry.Fatal("Fatal logger")
	entry.Panic("Panic logger")
}
```

단, Fatal은 exit 1으로 강제 종료되고, Panic은 Panic을 일으킵니다.