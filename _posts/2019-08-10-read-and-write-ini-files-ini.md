---
layout: post
title: Golang ini 파일 읽고 쓰는 ini 라이브러리 알아보기
---

오늘은 Golang으로 설정파일인 INI 파일 포맷을 읽고 쓸 수 있는 ini 라이브러리를 알아보려 합니다.

## ini 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 ini 패키지를 설치합니다.

```
go get gopkg.in/ini.v1
```

홈의 go 폴더에 ini 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"time"

	"gopkg.in/ini.v1"
)
```

fmt와 time 그리고 ini를 가져옵니다.

```go
func main() {
	cfg, err := ini.Load("setting.ini")
```

setting.ini 파일의 내용을 불러옵니다.

```go
	if err != nil {
		panic(err)
	}
```

예외처리를 합니다.

```go
	Bool, err := cfg.Section("types").Key("Bool").Bool()
	String := cfg.Section("types").Key("String").String()
	Int, err := cfg.Section("types").Key("Int").Int()
	Uint, err := cfg.Section("types").Key("Uint").Uint()
	TimeFormat, err := cfg.Section("types").Key("TimeFormat").TimeFormat(time.RFC3339)
	Time, err := cfg.Section("types").Key("Time").Time()
```

각종 타입으로 가져올 수 있습니다.

```go
	MustString := cfg.Section("Must").Key("MustString").MustString("default")
	MustBool := cfg.Section("Must").Key("MustBool").MustBool(true)
	MustInt := cfg.Section("Must").Key("MustInt").MustInt(10)
	MustUint := cfg.Section("Must").Key("MustUint").MustUint(10)
	MustTimeFormat := cfg.Section("Must").Key("MustTimeFormat").MustTimeFormat(time.RFC3339, time.Now())
	MustTime := cfg.Section("Must").Key("MustTime").MustTime(time.Now())
```

각종 타입으로 가져올 때에 기본 값을 지정할 수 있습니다.

```go
	In := cfg.Section("InTypes").Key("In").In("default", []string{"one", "two", "three"})
	InInt := cfg.Section("InTypes").Key("InInt").InInt(0, []int{10, 20, 30})
```

선택해서 가져올 수 있습니다.

```go
	RangeInt := cfg.Section("Range").Key("RangeInt").RangeInt(1, 0, 20)
```

시작과 최소, 최대값을 정하여 범위로 가져올 수 있습니다.

```go
	Strings := cfg.Section("delimiter").Key("Strings").Strings(",")
	Ints := cfg.Section("delimiter").Key("Ints").Ints(",")
	Uints := cfg.Section("delimiter").Key("Uints").Uints(",")
	Times := cfg.Section("delimiter").Key("Times").Times(",")
```

각종 타입으로 구분자로 분리하여 가져올 수 있습니다.

```go
	ValidInts := cfg.Section("Valid").Key("ValidInts").ValidInts(",")
	ValidTimes := cfg.Section("Valid").Key("ValidTimes").ValidTimes(",")
```

타입을 검사해서 원하는 타입만 골라서 가져올 수 있습니다.

```go
	StrictInts, err := cfg.Section("Strict").Key("INTS").StrictInts(",")
```

특정 타입외의 다른 타입이 들어오면 오류를 발생시킬 수 있습니다.

```go
	fmt.Print(Bool, String, Int, Uint, TimeFormat, Time,
		MustString, MustBool, MustInt, MustUint, MustTimeFormat, MustTime,
		In, InInt, RangeInt, Strings, Ints, Uints, Times,
		ValidInts, ValidTimes, StrictInts)
```

출력하여 setting.ini의 내용을 확인할 수 있습니다.

```go
	cfg.Section("").Key("Write_Value").SetValue("SetValue")
	cfg.SaveTo("setiing.ini.local")
}
```

ini 파일에 값을 쓸 수도 있습니다.

## 설정 파일

본 포스팅은 아래와 같이 설정 파일을 작성하고 진행하였습니다.

```ini
Write_Value = SetValue

[types]
Bool       = true
String     = hello
Float64    = 1
Int        = 1
Uint       = 1
TimeFormat = 2019-08-10T20:00:00+09:00
Time       = 2019-08-10T20:00:00+09:00

[Must]
MustString     = default
MustBool       = true
MustInt        = 10
MustUint       = 3
MustTimeFormat = 2019-08-10T22:29:57+09:00
MustTime       = 2019-08-10T22:29:57+09:00

[InTypes]
In    = one
InInt = 10

[Range]
RangeInt = 5

[delimiter]
Strings = hello,world
Ints    = 1,2,3
Uints   = 1,2,3
Times   = 2019-08-10T22:29:57+09:00,2019-08-11T22:29:57+09:00

[Valid]
ValidInts  = hello,world,1,2
ValidTimes = 2019-08-10T22:29:57+09:00 , 0001-00-00

[Strict]
INTS = 1,2,3,hello

```