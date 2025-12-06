---
layout: post
title: Golang 고루틴 워커 grpool 라이브러리 알아보기
---

오늘은 Golang으로 고루틴 워커를 만들어서 동시성 프로그래밍을 할 수 있는 grpool 라이브러리를 알아보려 합니다.

## grpool 설치

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

go get으로 grpool 패키지를 설치합니다.

```
go get github.com/ivpusic/grpool
```

홈의 go 폴더에 grpool 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/ivpusic/grpool"
)
```

fmt와 grpool을 가져옵니다.

```go
func main() {
	pool := grpool.NewPool(100, 50)
```

고루틴 워커를 만듭니다.

```go
	pool.WaitCount(100)
```

WaitAll 메소드를 호출하기 전에 얼마나 많은 수의 워커를 기다려야 되는 지 정합니다.

```go
	for i := 0; i < 100; i++ {
		value := i
		pool.JobQueue <- func() {
			defer pool.JobDone()
			fmt.Println(value)
		}
	}
```

각자의 반복문에서 작업이 완료되어 JobDone 메소드를 부르면 워커가 끝났다고 통지합니다.

```go
	pool.WaitAll()
```

모든 작업이 끝날 때까지 기다립니다.

```go
	defer pool.Release()
}
```

풀의 자원을 릴리즈합니다.