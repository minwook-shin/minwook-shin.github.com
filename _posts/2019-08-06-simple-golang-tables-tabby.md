---
layout: post
title: Golang 간단하게 표 그리는 Tabby 라이브러리 알아보기
---

오늘은 Golang으로 터미널에서 표를 그릴 수 있는 Tabby 라이브러리를 알아보려 합니다.

## Tabby 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 github에서 호스팅되고 있는 Tabby 패키지를 설치합니다.

```
go get github.com/cheynewallace/tabby
```

홈의 go 폴더에 Tabby 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"os"
	"text/tabwriter"

	"github.com/cheynewallace/tabby"
)
```

os와 tabwriter 그리고 tabby를 가져옵니다.

```go
func main() {
	table1 := tabby.New()
	table1.AddHeader("HEADER1", "HEADER2", "HEADER3")
	table1.AddLine("first", "second", "third")
	table1.Print()
```

헤더가 있는 테이블을 만들 수 있습니다.

```go
	table2 := tabby.New()
	table2.AddLine("LINE1:", "content_1", "content_1")
	table2.AddLine("LINE2:", "content_2", "content_2")
	table2.AddLine("LINE3:", "content_3", "content_3")
	table2.Print()
```

헤더가 없는 테이블을 만들 수 있습니다.

```go
	writer := tabwriter.NewWriter(os.Stdout, 0, 0, 2, '|', 0)
	table3 := tabby.NewCustom(writer)
	table3.AddLine("LINE1:", "content_1", "content_1")
	table3.AddLine("LINE2:", "content_2", "content_2")
	table3.AddLine("LINE3:", "content_3", "content_3")
	table3.Print()
}
```

tabwriter로 tabby 객체를 커스텀할 수 있습니다.

기본 tabby 객체는 0, 0, 2, ' ', 0 로 설정되어 있습니다.