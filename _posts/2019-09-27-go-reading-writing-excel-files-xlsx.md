---
layout: post
title: Golang 엑셀 파일을 쓰고 읽는 XLSX 라이브러리 알아보기
---

오늘은 Golang으로 마이크로소프트의 엑셀 파일을 쓰고 읽을 수 있는 XLSX 라이브러리를 알아보려 합니다.

## XLSX 설치

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

go get으로 XLSX 패키지를 설치합니다.

```
go get github.com/tealeg/xlsx
```

홈의 go 폴더에 XLSX 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/tealeg/xlsx"
)
```

fmt와 xlsx를 가져옵니다.

```go
func main() {
	file := xlsx.NewFile()
```

파일을 생성합니다.

```go
	sheet, err := file.AddSheet("Sheet1")
	if err != nil {
		panic(err)
	}
```

생성한 파일 객체에서 시트를 생성합니다.

```go
	row := sheet.AddRow()
	cell := row.AddCell()
	cell.Value = "VALUE"
```

Row를 생성하고, cell에 문자열을 기록합니다.

```go
	err = file.Save("test.xlsx")
	if err != nil {
		panic(err)
	}
```

위 파일 객체로 test라는 이름의 xlsx 엑셀 파일을 저장합니다.

```go
	file, err = xlsx.OpenFile("test.xlsx")
	if err != nil {
		panic(err)
	}
```

저장한 test.xlsx 파일을 엽니다.

```go
	for _, sheet := range file.Sheets {
		for _, row := range sheet.Rows {
			for _, cell := range row.Cells {
				fmt.Println(cell.String())
			}
		}
	}
}
```

test.xlsx의 sheet를 반복문으로 꺼내오고, row의 cell 값들을 전부 순차적으로 가져옵니다.