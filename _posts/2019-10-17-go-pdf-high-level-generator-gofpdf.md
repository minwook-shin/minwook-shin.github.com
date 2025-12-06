---
layout: post
title: Golang PDF 문서 생성하는 GoFPDF 라이브러리 알아보기
---

오늘은 Golang으로 구현된 PDF 문서를 고수준의 방식의 생성기로 생성할 수 있는 GoFPDF 라이브러리를 알아보려 합니다.

## GoFPDF 설치

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

go get으로 GoFPDF 패키지를 설치합니다.

```
go get github.com/jung-kurt/gofpdf
```

홈의 go 폴더에 GoFPDF 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import "github.com/jung-kurt/gofpdf"
```

gofpdf를 가져옵니다.

```go
func main() {
	pdf := gofpdf.New("portrait", "mm", "A4", "")
```

새로운 Fpdf 객체를 만듭니다.

```go
	pdf.AddPage()
```

Fpdf 객체에서 문서의 페이지를 추가합니다.

```go
	pdf.SetAuthor("author", true)
```

Fpdf 객체에서 문서의 작성자를 정의하여 파일 정보에서 작성자를 볼 수 있게 합니다.

```go
	pdf.SetTitle("title", true)
```

Fpdf 객체에서 문서의 제목을 정의하여 파일 정보에서 제목을 볼 수 있게 합니다.

```go
	pdf.SetFont("Arial", "B", 16)
```

Fpdf 객체에서 문서의 폰트를 설정합니다.

```go
	pdf.Cell(50, 20, "Hello, world!")
```

Fpdf 객체에서 문서의 셀을 생성하고 문자열을 기록합니다.

```go
	err := pdf.OutputFileAndClose("test.pdf")
	if err != nil {
		panic(err)
	}
}
```

test.pdf 파일로 저장하고 객체를 닫습니다.