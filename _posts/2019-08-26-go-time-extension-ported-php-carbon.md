---
layout: post
title: Golang PHP 시간 관련 carbon 포팅한 Carbon 라이브러리 알아보기
---

오늘은 Golang으로 PHP의 날짜와 시간을 제어할 수 있는 Carbon을 포팅한 라이브러리를 알아보려 합니다.

## Carbon 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 Carbon 패키지를 설치합니다.

```
go get github.com/uniplaces/carbon
```

홈의 go 폴더에 Carbon 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"time"

	"github.com/uniplaces/carbon"
)
```

fmt와 time, 그리고 carbon을 가져옵니다.

```go
func main() {
	fmt.Println(carbon.Now().DateTimeString())
```

현재 시간을 반환할 수 있습니다.

```go
	today, _ := carbon.NowInLocation("Asia/Seoul")
	fmt.Println(today)
```

서울 시간대로 현재 날짜를 반환합니다.

```go
	carbonTime := today.AddYear()
	fmt.Println(carbonTime)
	carbonTime = today.AddYears(1)
	fmt.Println(carbonTime)

	carbonTime = today.AddMonth()
	fmt.Println(carbonTime)
	carbonTime = today.AddMonths(1)
	fmt.Println(carbonTime)

	carbonTime = today.AddWeekday()
	fmt.Println(carbonTime)
	carbonTime = today.AddWeekdays(1)
	fmt.Println(carbonTime)

	carbonTime = today.AddHour()
	fmt.Println(carbonTime)
	carbonTime = today.AddMinute()
	fmt.Println(carbonTime)
	carbonTime = today.AddSecond()
	fmt.Println(carbonTime)
```

Add 접두사가 붙으면 시간이나 날짜를 더해줍니다.

```go
	carbonTime = today.SubYear()
	fmt.Println(carbonTime)
	carbonTime = today.SubYears(1)
	fmt.Println(carbonTime)

	carbonTime = today.SubMonth()
	fmt.Println(carbonTime)
	carbonTime = today.SubMonths(1)
	fmt.Println(carbonTime)

	carbonTime = today.SubWeekday()
	fmt.Println(carbonTime)
	carbonTime = today.SubWeekdays(1)
	fmt.Println(carbonTime)

	carbonTime = today.SubHour()
	fmt.Println(carbonTime)
	carbonTime = today.SubMinute()
	fmt.Println(carbonTime)
	carbonTime = today.SubSecond()
	fmt.Println(carbonTime)
```

Sub 접두사가 붙으면 시간이나 날짜를 뺍니다.

```go
	testDate, _ := carbon.CreateFromDate(2019, 8, 25, "Asia/Seoul")
```

서울의 시간대를 기준과 지정한 날짜로 Carbon 객체를 생성합니다.

```go
	fmt.Println(testDate.IsWeekday())
	fmt.Println(testDate.IsWeekend())

	fmt.Println(testDate.IsPast())
	fmt.Println(testDate.IsFuture())
```

생성한 Carbon 객체로 주일인지 주말인지를 알 수 있고, 과거인지 미래인지도 알수 있습니다.

```go
	seoul, _ := carbon.CreateFromDate(2019, 8, 25, "Asia/Seoul")
	london, _ := carbon.CreateFromDate(2019, 8, 25, "Europe/London")
	fmt.Println(london.DiffInHours(seoul, true))
```

서로 두 객체의 시간 차이를 계산할 수도 있습니다.

```go
	parsed, _ := carbon.Parse(carbon.DateFormat, "2019-08-25", "Asia/Seoul")
	fmt.Println(parsed)
```

문자열을 파싱하여 반환할 수 있습니다.

```go
	EndOfWeekTime, _ := carbon.Create(2019, 8, 25, 12, 0, 0, 0, "Asia/Seoul")
	fmt.Println(EndOfWeekTime.EndOfWeek())
```

마지막 요일의 날짜를 반환하는 것처럼 반환하는 메소드들도 존재합니다.

```go
	nextWednesday, _ := carbon.Create(2019, 8, 25, 12, 0, 0, 0, "Asia/Seoul")
	fmt.Println(nextWednesday.Next(time.Wednesday))
}
```

다음 요일이 언제인지도 알 수 있습니다.
