---
layout: post
title: Golang 시간 툴킷 Now 라이브러리 알아보기
---

오늘은 Golang으로 날짜 문자열을 파싱하거나 날짜를 계산하는 시간 툴킷인 Now 라이브러리를 알아보려 합니다.

## Now 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 Now 패키지를 설치합니다.

```
go get -u github.com/jinzhu/now
```

홈의 go 폴더에 Now 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"time"

	"github.com/jinzhu/now"
)
```

fmt, time 그리고 now를 가져옵니다.

```go
func main() {
	timeNow := time.Now()
	fmt.Println(timeNow)
```

현재 시간을 반환할 수 있습니다.

```go
	timeNow = now.New(time.Date(2000, 1, 1, 00, 00, 00, 0, time.Now().Location())).Local()
	fmt.Println(timeNow)
```

원하는 시간을 time.Time 타입으로 지정하고 반환할 수 있습니다.

```go
	timeNow = now.BeginningOfMinute()
	fmt.Println(timeNow)
	timeNow = now.BeginningOfHour()
	fmt.Println(timeNow)
	timeNow = now.BeginningOfDay()
	fmt.Println(timeNow)
	timeNow = now.BeginningOfWeek()
	fmt.Println(timeNow)
	timeNow = now.BeginningOfMonth()
	fmt.Println(timeNow)
	timeNow = now.BeginningOfQuarter()
	fmt.Println(timeNow)
	timeNow = now.BeginningOfYear()
	fmt.Println(timeNow)

	timeNow = now.EndOfMinute()
	fmt.Println(timeNow)
	timeNow = now.EndOfHour()
	fmt.Println(timeNow)
	timeNow = now.EndOfDay()
	fmt.Println(timeNow)
	timeNow = now.EndOfWeek()
	fmt.Println(timeNow)
	timeNow = now.EndOfMonth()
	fmt.Println(timeNow)
	timeNow = now.EndOfQuarter()
	fmt.Println(timeNow)
	timeNow = now.EndOfYear()
	fmt.Println(timeNow)
```

미리 만들어둔 Now 객체로 위와 같은 메소드를 사용할 수 있습니다.

지정한 시간을 바탕으로 특정 조건의 시작점과 끝점을 볼 수 있습니다.

```go
	timeParser, err := now.Parse("2019")
	if err != nil {
		panic(err)
	}
	fmt.Println(timeParser)
	timeParser, err = now.Parse("2019-08")
	if err != nil {
		panic(err)
	}
	fmt.Println(timeParser)
	timeParser, err = now.Parse("2019-08-27")
	if err != nil {
		panic(err)
	}
	fmt.Println(timeParser)
	timeParser, err = now.Parse("2019-08-27 12:00")
	if err != nil {
		panic(err)
	}
	fmt.Println(timeParser)
	timeParser, err = now.Parse("2019-08-27 12:00:00")
	if err != nil {
		panic(err)
	}
	fmt.Println(timeParser)
	timeParser, err = now.Parse("12:00")
	if err != nil {
		panic(err)
	}
	fmt.Println(timeParser)
```

문자열에서 시간을 파싱할 수 있습니다.

기입되지 않은 부분을 현재 시간으로 채울 수 있습니다.

만약 파싱할 수 없으면 오류를 err 변수로 반환합니다.

```go
	timeParser = now.MustParse("08-27")
	fmt.Println(timeParser)
	timeParser = now.MustParse("2019-08-27")
	fmt.Println(timeParser)
}
```

MustParse는 똑같이 파싱하지만, 파싱할 수 없는 상황에서 패닉을 일으킵니다.