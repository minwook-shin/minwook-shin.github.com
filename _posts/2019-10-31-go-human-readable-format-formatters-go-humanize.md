---
layout: post
title: Golang 사람이 보기 좋게 포맷팅하는 go-humanize 라이브러리 알아보기
---

오늘은 Golang으로 시간, 크기 등을 사람이 보기 좋게 포맷팅할 수 있는 go-humanize 라이브러리를 알아보려 합니다.

## go-humanize 설치

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

go get으로 go-humanize 패키지를 설치합니다.

```
go get github.com/dustin/go-humanize
```

홈의 go 폴더에 go-humanize 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"time"

	"github.com/dustin/go-humanize"
	"github.com/dustin/go-humanize/english"
)
```

fmt, time 그리고 go-humanize를 가져옵니다.

```go
func main() {

	mb := humanize.Bytes(5012345)
	fmt.Println(mb)

	t := time.Date(2018, time.November, 1, 0, 0, 0, 0, time.UTC)
	time := humanize.Time(t)
	fmt.Println(time)

	ordinal := humanize.Ordinal(99)
	fmt.Println(ordinal)
```

크기, 시간, 순위를 사람이 보기 좋게 포맷팅합니다.

```go
	comma := humanize.Comma(123456789)
	fmt.Println(comma)
```

쉼표를 첨부해서 숫자 단위를 보기 좋게 포맷팅합니다.

```go
	fmt.Println(humanize.Ftoa(2.00))
```

부동 소수점에서 0을 제거하여 포맷팅합니다.

```go
	fmt.Println(english.PluralWord(1, "keyboard", ""))
	fmt.Println(english.PluralWord(2, "keyboard", ""))

	fmt.Println(english.Plural(1, "keyboard", ""))
	fmt.Println(english.Plural(2, "keyboard", ""))
```

단위에 따라 단어의 복수형을 사용하면서 포맷팅합니다.

```go
	fmt.Println(english.WordSeries([]string{"hello", "world"}, "and"))
}
```

단어들을 이어서 사람이 보기 좋게 포맷팅합니다.