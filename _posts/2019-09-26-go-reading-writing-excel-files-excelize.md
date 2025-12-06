---
layout: post
title: Golang 엑셀 파일을 쓰고 읽는 Excelize 라이브러리 알아보기
---

오늘은 Golang으로 마이크로소프트의 엑셀 파일을 쓰고 읽을 수 있는 Excelize 라이브러리를 알아보려 합니다.

## Excelize 설치

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

go get으로 Excelize 패키지를 설치합니다.

```
go get github.com/360EntSecGroup-Skylar/excelize
```

홈의 go 폴더에 Excelize 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/360EntSecGroup-Skylar/excelize"
)
```

fmt와 excelize를 가져옵니다.

```go
func main() {
	file := excelize.NewFile()
	sheet := file.NewSheet("Sheet1")
	file.SetActiveSheet(sheet)
```

새로운 파일과 시트를 만듭니다.

```go
	file.SetCellValue("Sheet1", "A1", "VALUE")
```

Sheet1 시트의 A1 셀 위치에 VALUE라고 기록합니다.

```go
	err := file.SaveAs("TEST_1.xlsx")
	if err != nil {
		fmt.Println(err)
	}
```

위 변경사항을 TEST_1라는 이름의 엑셀 파일로 저장합니다.

```go
	items := map[string]string{"A2": "Fisrt", "A3": "Second", "B1": "Java", "C1": "Python"}
	values := map[string]int{"B2": 2, "C2": 4, "B3": 3, "C3": 6}
```

차트를 만들기 위한 데이터를 준비합니다.

```go
	sheet = file.NewSheet("Sheet2")
	file.SetActiveSheet(sheet)
```

Sheet2 시트를 만듭니다.

```go
	for key, value := range items {
		file.SetCellValue("Sheet2", key, value)
	}
	for key, value := range values {
		file.SetCellValue("Sheet2", key, value)
	}
```

Sheet2 시트에 위 데이터를 키 값에 맞게 셀에 기록합니다.

```go
	err = file.AddChart("Sheet2", "D1", `{"type":"col3DClustered","series":[
		{"name":"Sheet2!$A$2","categories":"Sheet2!$B$1:$C$1","values":"Sheet2!$B$2:$C$2"},
		{"name":"Sheet2!$A$3","categories":"Sheet2!$B$1:$C$1","values":"Sheet2!$B$3:$C$3"}],
		"title":{"name":"Test Chart"}}`)
	if err != nil {
		fmt.Println(err)
		return
	}
	err = file.SaveAs("TEST_1.xlsx")
	if err != nil {
		fmt.Println(err)
	}
```

셀의 위치를 지정하여 데이터를 가져오고 해당 데이터를 기반으로 차트를 생성합니다.

차트가 생성되면 파일을 다시 저장합니다.

```go
	file, err = excelize.OpenFile("TEST_1.xlsx")
	if err != nil {
		fmt.Println(err)
		return
	}
```

저장된 파일을 엽니다.

```go
	value, err := file.GetCellValue("Sheet1", "A1")
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(value)
```

Sheet1 시트의 A1 셀 위치에 VALUE라고 기록되어 있으므로 VALUE가 출력됩니다.

```go
	rows, err := file.GetRows("Sheet1")
	for _, row := range rows {
		for _, value := range row {
			fmt.Println(value)
		}
	}
}

```

모든 rows를 가져오는 방식으로 Sheet1의 모든 내용을 가져옵니다.
