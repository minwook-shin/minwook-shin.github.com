---
layout: post
title: Golang 고루틴 ants 라이브러리 알아보기
---

오늘은 Golang으로 고루틴 풀 동시성 프로그래밍을 할 수 있는 ants 라이브러리를 알아보려 합니다.

## ants 설치

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

go get으로 ants 패키지를 설치합니다.

```
go get -u github.com/panjf2000/ants
```

홈의 go 폴더에 ants 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"sync"
	"sync/atomic"

	"github.com/panjf2000/ants"
)
```

fmt와 sync 그리고 ants를 가져옵니다.

```go
var sum int32
```

고루틴으로 수를 더해 줄 int32 변수를 만듭니다.

```go
func helloFunc() {
	fmt.Println("Hello World!")
}
```

Hello World!라고 출력하는 함수를 만듭니다.

```go
func intFunc(i interface{}) {
	n := i.(int32)
	atomic.AddInt32(&sum, n)
	fmt.Println(n)
}
```

델타를 추가하고 새로운 값을 sum에 반환하는 함수를 만듭니다.

```go
func main() {
	var waitGroup sync.WaitGroup
```

고루틴이 끝날 때까지 대기할 수 있는 waitGroup을 만듭니다.

```go
	for i := 0; i < 100; i++ {
		waitGroup.Add(1)
		_ = ants.Submit(func() {
			helloFunc()
			waitGroup.Done()
		})
	}
	waitGroup.Wait()
```

waitGroup의 Add로 고루틴을 추가하고, Done으로 끝났다는 것을 통지하며 for문이 끝날 때까지 Wait로 기다립니다.

```go
	poolFunc, _ := ants.NewPoolWithFunc(10, func(i interface{}) {
		intFunc(i)
		waitGroup.Done()
	})
```

ants의 풀을 만들어서 함수를 수행하고, Done으로 끝났다는 것을 통지합니다.

```go
	for i := 0; i < 100; i++ {
		waitGroup.Add(1)
		_ = poolFunc.Invoke(int32(i))
	}
	waitGroup.Wait()
	fmt.Println(sum)
```

waitGroup의 Add로 고루틴을 추가하고, 해당 ants의 풀에 int32 변수를 넣습니다.

for문이 끝날 때까지 Wait로 기다리고, intFunc 함수에 의하여 모인 sum 변수의 값을 출력합니다.

```go
	defer ants.Release()
	defer poolFunc.Release()
}
```

defer 키워드로 인하여 마지막에 풀을 닫고 종료됩니다.