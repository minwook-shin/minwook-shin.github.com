---
layout: post
title: Golang 이메일 본문 HTML 코드 생성하는 Hermes 라이브러리 알아보기
---

오늘은 Golang으로 이메일에 보낼 수 있는 HTML 코드를 생성해주는 Hermes 라이브러리를 알아보려 합니다.

## Hermes 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 Hermes 패키지를 설치합니다.

```
go get github.com/matcornic/hermes/v2 
```

홈의 go 폴더에 Hermes 소스코드와 패키지 파일이 생성됩니다.

```
export GO111MODULE=on
```

만약 설치되지 않는다면 설치하기 전에 go module을 설정해봅니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"io/ioutil"

	"github.com/matcornic/hermes/v2"
)
```

fmt, ioutil 그리고 hermes를 가져옵니다.

```go
func main() {
	hermesGenerator := hermes.Hermes{
		Product: hermes.Product{
			Name:        "minwook_test",
			Link:        "https://example.com/",
			Logo:        "https://octodex.github.com/images/original.png",
			Copyright:   "Copyright © 2019 test. All rights reserved.",
			TroubleText: "TroubleText",
		},
	}
```

이메일에 헤더와 바닥글에 보일 정보를 기입합니다.

```go
	email := hermes.Email{
		Body: hermes.Body{
			Name: "Consumer",
			Intros: []string{
				"Welcome to example! Intros text",
			},
			Actions: []hermes.Action{
				{
					Instructions: "Instructions, click here:",
					Button: hermes.Button{
						Text: "Click Text",
						Link: "https://example.com/",
					},
				},
			},
			Outros: []string{
				"Good bye, Outros.",
			},
			Greeting:  "Hello, world!",
			Signature: "Signature text",
			Table: hermes.Table{
				Data: [][]hermes.Entry{
					{
						{Key: "language", Value: "Golang"},
						{Key: "Description", Value: "Open source programming language"},
						{Key: "ETC", Value: ""},
					},
				},
			},
			Dictionary: []hermes.Entry{
				{Key: "fisrt", Value: "first value"},
				{Key: "second", Value: "second value"},
			},
		},
	}
```

이메일의 본문에 이름, 인삿말, 버튼, 사인, 테이블과 같은 것들을 배치할 수 있습니다.

```go
	emailText, err := hermesGenerator.GeneratePlainText(email)
	if err != nil {
		panic(err)
	}
	fmt.Println(emailText)
```

위에 작성한 이메일 본문에 대한 내용들을 순수한 문자열로 나타낼 수 있습니다.

```go
	emailBody, err := hermesGenerator.GenerateHTML(email)
	if err != nil {
		panic(err)
	}

	err = ioutil.WriteFile("index.html", []byte(emailBody), 0644)
	if err != nil {
		panic(err)
	}
}

```

위에 작성한 이메일 본문을 HTML로 생성하여 최종적으로는 파일로 내보낼 수 있습니다.