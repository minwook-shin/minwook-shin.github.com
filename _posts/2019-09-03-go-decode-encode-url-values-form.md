---
layout: post
title: Golang url.Values 디코드 인코드 가능한 form 라이브러리 알아보기
---

오늘은 Golang으로 url.Values에서 디코드하여 go언어에서 사용할 수 있는 변수 값으로 만들 수 있고, 다시 url.Values 인코드할 수도 있는 form 라이브러리를 알아보려 합니다.

## form 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 form 패키지를 설치합니다.

```
go get -u github.com/go-playground/form
```

홈의 go 폴더에 form 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"net/url"

	"github.com/go-playground/form"
)
```

fmt와 url, 그리고 form을 가져옵니다.

```go
type Test struct {
	Id           string `form:"id"`
	Age          uint8  `form:"age"`
	Email        string `form:"email"`
	Active       bool   `form:"active"`
	MapExample   map[string]string
	ArrayExample []string
}
```

구조체를 만들어서 url.Values를 변수로 만들기 위한 틀을 작성합니다.

```go
var decoder *form.Decoder

var encoder *form.Encoder
```

Decoder와 Encoder를 선언합니다.

```go
func getValues() url.Values {
	return url.Values{
		"id":              []string{"test_id"},
		"age":             []string{"20"},
		"email":           []string{"test@example.com"},
		"active":          []string{"true"},
		"MapExample[key]": []string{"value"},
		"ArrayExample[0]": []string{"value"},
	}
}
```

웹에서 form이 들어와있을 때를 가정한 반환 값을 가진 함수를 만들어줍니다.

```go
func main() {
	decoder = form.NewDecoder()

	urlValues := getValues()

	var entity Test
	err := decoder.Decode(&entity, urlValues)
	if err != nil {
		panic(err)
	}

	fmt.Println(entity)
```

외부에서 들어오는 형식을 구조체에 맞추어 Decode해주면 변수로서 사용할 수 있습니다.

```go
	encoder = form.NewEncoder()

	urlValues, err = encoder.Encode(&entity)
	if err != nil {
		panic(err)
	}
	fmt.Println(urlValues)
}
```

반대로 객체로 만들어 둔 상태로 Encode하면 다시 url.Values로 반환됩니다.