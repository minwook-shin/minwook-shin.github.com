---
layout: post
title: Golang 날짜 문자열 파싱하는 Go Date Parser 라이브러리 알아보기
---

오늘은 Golang으로 다양한 날짜 문자열을 파싱할 수 있는 Go Date Parser 라이브러리를 알아보려 합니다.

## Go Date Parser 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 Go Date Parser 패키지를 설치합니다.

```
github.com/araddon/dateparse
```

홈의 go 폴더에 Go Date Parser 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"time"

	"github.com/araddon/dateparse"
)
```

fmt와 time 그리고 dateparse를 가져옵니다.

```go
func main() {
	timeVar, err := dateparse.ParseAny("2019/08/26")
	if err != nil {
		panic(err)
	}
	fmt.Println(timeVar)

	timeVar, err = dateparse.ParseStrict("2019/08/26")
	if err != nil {
		panic(err)
	}
	fmt.Println(timeVar)
```

문자열을 가지고 날짜를 파싱합니다.

```go
	// timeVar = dateparse.MustParse("hello")
```

MustParse는 오류를 발생시키지 않고 파싱에 실패하면 패닉을 발생합니다.

```go
	layout, err := dateparse.ParseFormat("Aug 8, 2019 00:00:00 PM")
	if err != nil {
		panic(err)
	}
	fmt.Println(layout)
```

ParseFormat은 파싱할 레이아웃 문자열을 반환합니다.

```go
	loc, err := time.LoadLocation("Asia/Seoul")
	if err != nil {
		panic(err)
	}
```

Location을 생성합니다.

```go
	timeVar, err = dateparse.ParseIn("2019/08/26", loc)
	if err != nil {
		panic(err)
	}
	fmt.Println(timeVar)

	time.Local = loc
```

Location을 기반으로 날짜 문자열을 파싱하거나, 시간대 설정을 전역으로 설정할 수 있습니다.

```go
	timeVar, err = dateparse.ParseLocal("Aug 26, 2019 00:00:00 PM")
	if err != nil {
		panic(err)
	}
	fmt.Println(timeVar)
	timeVar, err = dateparse.ParseLocal("Aug 26, 2019")
	if err != nil {
		panic(err)
	}
	fmt.Println(timeVar)
	timeVar, err = dateparse.ParseLocal("Mon Aug 26 00:00:00 2019")
	if err != nil {
		panic(err)
	}
	fmt.Println(timeVar)
	timeVar, err = dateparse.ParseLocal("August 26, 2019 00:00am")
	if err != nil {
		panic(err)
	}
	fmt.Println(timeVar)
	timeVar, err = dateparse.ParseLocal("August 26, 2019, 00:00:00")
	if err != nil {
		panic(err)
	}
	fmt.Println(timeVar)
	timeVar, err = dateparse.ParseLocal("August 26, 2019")
	if err != nil {
		panic(err)
	}
	fmt.Println(timeVar)
	timeVar, err = dateparse.ParseLocal("August 26th, 2019")
	if err != nil {
		panic(err)
	}
	fmt.Println(timeVar)
	timeVar, err = dateparse.ParseLocal("26 Aug 2019, 00:00")
	if err != nil {
		panic(err)
	}
	fmt.Println(timeVar)
	timeVar, err = dateparse.ParseLocal("26 Aug 2019 00:00")
	if err != nil {
		panic(err)
	}
	fmt.Println(timeVar)
	timeVar, err = dateparse.ParseLocal("26 Aug 2019")
	if err != nil {
		panic(err)
	}
	fmt.Println(timeVar)
	timeVar, err = dateparse.ParseLocal("26 August 2019")
	if err != nil {
		panic(err)
	}
	fmt.Println(timeVar)
	timeVar, err = dateparse.ParseLocal("2019-Aug-26")
	if err != nil {
		panic(err)
	}
	fmt.Println(timeVar)
	timeVar, err = dateparse.ParseLocal("2019/08/26")
	if err != nil {
		panic(err)
	}
	fmt.Println(timeVar)
	timeVar, err = dateparse.ParseLocal("2019/08/26 00:00")
	if err != nil {
		panic(err)
	}
	fmt.Println(timeVar)
	timeVar, err = dateparse.ParseLocal("2019/08/26 00:00:00")
	if err != nil {
		panic(err)
	}
	fmt.Println(timeVar)
	timeVar, err = dateparse.ParseLocal("2019-08-26")
	if err != nil {
		panic(err)
	}
	fmt.Println(timeVar)
	timeVar, err = dateparse.ParseLocal("20190626")
	if err != nil {
		panic(err)
	}
	fmt.Println(timeVar)
	timeVar, err = dateparse.ParseLocal("20190826000000")
	if err != nil {
		panic(err)
	}
	fmt.Println(timeVar)
}
```

위에서 미리 설정한 로컬 시간을 기준으로 레이아웃을 감지하여 문자열을 날짜로 파싱합니다.