---
layout: post
title: Golang 터미널에서 프로그레스 바 구현하는 uiprogress 라이브러리 알아보기
---

오늘은 Golang으로 터미널에서 프로그레스 바(진행바)를 구현할 수 있는 uiprogress 라이브러리를 알아보려 합니다.

## uiprogress 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 github에서 호스팅되고 있는 uiprogress 패키지를 설치합니다.

```
go get -v github.com/gosuri/uiprogress
```

홈의 go 폴더에 uiprogress 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"math/rand"
	"sync"
	"time"

	"github.com/gosuri/uiprogress"
)
```

rand, sync, time 그리고 uiprogress를 가져옵니다.

```go
func main() {
	uiprogress.Start()
	bar := uiprogress.AddBar(100)

	bar.AppendCompleted()
	bar.PrependElapsed()

	for bar.Incr() {
		time.Sleep(time.Millisecond * 10)
	}
```

프로그레스 바를 생성하고 기본 progress container로 설정합니다.

AppendCompleted, PrependElapsed에 의해서 진행도와 진행 시간이 표시될 수 있습니다.

```go
	customBar := uiprogress.AddBar(4)

	customBar.PrependFunc(func(bar *uiprogress.Bar) string {
		return []string{"downloading", "removing", "updating", "running"}[bar.Current()-1]
	})

	for customBar.Incr() {
		time.Sleep(time.Millisecond * 500)
	}
```

PrependFunc 데코레이터 함수에 의해 프로그레스 바를 커스터마이징을 할 수 있습니다.

```go
	var wg sync.WaitGroup

	bar1 := uiprogress.AddBar(100).AppendCompleted().PrependElapsed()
	wg.Add(1)
	go func() {
		defer wg.Done()
		for bar1.Incr() {
			time.Sleep(time.Millisecond * 50)
		}
	}()
```

WaitGroup으로 동시성으로 만들어서 Incr 함수로 값을 계속 증가시키며 프로그레스 바를 만들 수 있습니다.

```go
	bar2 := uiprogress.AddBar(100).PrependElapsed().AppendCompleted()
	wg.Add(1)
	go func() {
		defer wg.Done()
		for i := 1; i <= bar2.Total; i++ {
			bar2.Set(i)
			time.Sleep(time.Millisecond * 50)
		}
	}()

	wg.Wait()
}
```

또는 Set 함수로 프로그레스 바의 현재 진행도를 설정해주면서 값을 증가시키며 프로그레스 바를 만들 수 있습니다.