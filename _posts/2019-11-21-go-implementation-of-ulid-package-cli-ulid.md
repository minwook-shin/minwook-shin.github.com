---
layout: post
title: Golang ULID 구현하는 ULID 라이브러리 알아보기
---

오늘은 Golang으로 Universally Unique Lexicographically Sortable Identifier 구현하는 ULID 라이브러리를 알아보려 합니다.

## ULID 설치

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

go get으로 ULID 패키지를 설치합니다.

```
go get github.com/oklog/ulid
```

홈의 go 폴더에 ULID 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"math/rand"
	"time"

	"github.com/oklog/ulid"
)
```

fmt, math, time 그리고 ulid를 가져옵니다.

```go
func main() {
	localTime := time.Unix(1000000, 0)
	unixTime := localTime.UnixNano()
```

로컬 시간을 기반으로 1970년 1월 1일 이후의 유닉스 시간을 생성합니다.

해당 유닉스 시간은 int64 타입입니다.

```go
	randSource := rand.NewSource(unixTime)
```

유닉스 시간으로 랜덤 소스를 생성합니다.

```go
	random := rand.New(randSource)
```

랜덤 소스로 Rand를 생성합니다.

```go
	entropy := ulid.Monotonic(random, 0)
```

ULID 타임 스탬프로 엔트로피 소스를 생성합니다.

```go
	ulid := ulid.MustNew(ulid.Timestamp(localTime), entropy)
	fmt.Println(ulid)
}
```

만들어둔 초 단위 시간 스탬프와 엔트로피 소스로 ULID를 생성하여 출력합니다.