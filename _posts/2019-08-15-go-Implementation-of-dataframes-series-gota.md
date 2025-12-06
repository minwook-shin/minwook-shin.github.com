---
layout: post
title: Golang DataFrames를 구현하는 Gota 라이브러리 알아보기
---

오늘은 Golang으로 2차원 테이블인 DataFrames, Series를 구현한 Gota 라이브러리를 알아보려 합니다.

## Gota 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 Gota 패키지를 설치합니다.

```
go get github.com/go-gota/gota/dataframe
```

홈의 go 폴더에 Gota 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"strings"

	"github.com/go-gota/gota/dataframe"
	"github.com/go-gota/gota/series"
)
```

fmt, strings 그리고 gota의 dataframe, series를 가져옵니다.

```go
func main() {
	df := dataframe.New(
		series.New([]string{"b", "a"}, series.String, "STRING"),
		series.New([]int{1, 2}, series.Int, "INT"),
		series.New([]float64{1.0, 2.0}, series.Float, "FLOAT"),
	)
	fmt.Println(df)
```

series를 만들고 dataframe 생성자로 DataFrame 객체를 생성합니다.

```go
	df2 := dataframe.LoadRecords(
		[][]string{
			[]string{"STRING", "INT", "FLOAT", "BOOL"},
			[]string{"a", "1", "1.0", "true"},
			[]string{"b", "2", "2.0", "false"},
		},
	)
	fmt.Println(df2)
```

이미 만들어진 슬라이스 기반으로 DataFrame 객체를 생성합니다.

```go
	type User struct {
		STRING  string
		INT     int
		FLOAT   float64
	}
	users := []User{
		{"a", 1, 1.0, true},
		{"b", 2, 2.0, true},
	}
	df3 := dataframe.LoadStructs(users)
	fmt.Println(df3)
```

구조체로 DataFrame 객체를 생성하면서 구조체 이름에 없는 데이터는 객체에 포함하지 못하게 할 수 있게 할 수 있습니다.

```go
	df4 := dataframe.LoadRecords(
		[][]string{
			[]string{"STRING", "INT", "FLOAT", "BOOL"},
			[]string{"a", "1", "1.0", "true"},
			[]string{"b", "2", "2.0", "false"},
		},
		dataframe.DetectTypes(false),
		dataframe.DefaultType(series.Float),
		dataframe.WithTypes(map[string]series.Type{
			"STRING": series.String,
			"BOOL":   series.Bool,
		}),
	)
	fmt.Println(df4)
```

변수 타입을 자동으로 할 수 있지만, 옵션으로 기본 타입을 설정하거나 특정 시리즈에 타입을 부여할 수 있습니다.

```go
	df5 := dataframe.LoadMaps(
		[]map[string]interface{}{
			map[string]interface{}{
				"STRING": "a",
				"INT":    1,
				"FLOAT":  1.0,
				"BOOL":   true,
			},
			map[string]interface{}{
				"STRING": "b",
				"INT":    2,
				"FLOAT":  2.0,
				"BOOL":   true,
			},
		},
	)

	fmt.Println(df5)
```

map 인터페이스로도 불러와서 DataFrame 객체를 생성할 수 있습니다.

```go
	csvStr := `
string,int,float,bool
"a",1,1.0,true
"b",2,2.0,false
`
	df6 := dataframe.ReadCSV(strings.NewReader(csvStr))

	fmt.Println(df6)
```

```go
	jsonStr := `[{"INT":1,"FLOAT":1.0},{"STRING":"b","INT":2,"FLOAT":2.0}]`
	df7 := dataframe.ReadJSON(strings.NewReader(jsonStr))

	fmt.Println(df7)
}
```

CSV, JSON 형식으로 이루어져 있는 문자열을 기반으로 DataFrame 객체를 생성할 수 있습니다.