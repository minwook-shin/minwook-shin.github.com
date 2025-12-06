---
layout: post
title: Golang 터미널 스피너 Spinner 라이브러리 알아보기
---

오늘은 Golang으로 터미널에 스피너나 진행률을 표시할 수 있는 Spinner 라이브러리를 알아보려 합니다.

## Spinner 설치

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

go get으로 Spinner 패키지를 설치합니다.

```
go get github.com/briandowns/spinner
```

홈의 go 폴더에 Spinner 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"time"

	"github.com/briandowns/spinner"
)
```

fmt, time 그리고 spinner를 가져옵니다.

```go
func main() {
	spinnerInstance := spinner.New(spinner.CharSets[9], 100*time.Millisecond)
	spinnerInstance.Start()
	time.Sleep(2 * time.Second)
	spinnerInstance.Stop()
```

원하는 Character Sets로 Spinner 객체를 생성하고, 인디케이터로 스피너를 작동/중지합니다.

```go
	spinnerInstance.Reverse()
	spinnerInstance.Restart()
	time.Sleep(2 * time.Second)
	spinnerInstance.Stop()
```

스피너를 거꾸로 작동합니다.

```go
	spinnerInstance.UpdateCharSet(spinner.CharSets[36])
	spinnerInstance.Restart()
	time.Sleep(2 * time.Second)
	spinnerInstance.Stop()
```

원하는 Character Sets로 갱신합니다.

```go
	spinnerInstance.UpdateSpeed(1 * time.Millisecond)
	spinnerInstance.Restart()
	time.Sleep(2 * time.Second)
	spinnerInstance.Stop()
```

원하는 속도로 갱신합니다.

```go
	cs := []string{"o", "O"}
	spinnerInstance = spinner.New(cs, 100*time.Millisecond)
	spinnerInstance.Start()
	time.Sleep(2 * time.Second)
	spinnerInstance.Stop()
```

기존 Character Sets를 대신하여 Spinner 객체를 생성합니다.

```go
	spinnerInstance.Prefix = "prefix: "
	spinnerInstance.Suffix = " :suffix"
	spinnerInstance.Start()
	time.Sleep(2 * time.Second)
	spinnerInstance.Stop()
```

Spinner에 접두사와 접미사에 원하는 텍스트를 넣습니다.

```go
	spinnerInstance.Color("blue", "bold")
	spinnerInstance.Start()
	time.Sleep(2 * time.Second)
	spinnerInstance.Stop()
```

Spinner에 색상과 스타일을 지정합니다.

```go
	cs = spinner.GenerateNumberSequence(10)
	spinnerInstance = spinner.New(cs, 100*time.Millisecond)
	spinnerInstance.Start()
	time.Sleep(2 * time.Second)
	fmt.Println(spinnerInstance.Active())
	spinnerInstance.Stop()
```

Active로 현재 Spinner 객체가 작동 중인지 확인합니다.

```go
	spinnerInstance = spinner.New(spinner.CharSets[9], 100*time.Millisecond)
	spinnerInstance.FinalMSG = "first line\nsecond line\n"
	spinnerInstance.Start()
	time.Sleep(2 * time.Second)
	spinnerInstance.Stop()
}
```

Spinner 객체가 동작을 끝내면 출력할 문자열을 지정합니다.