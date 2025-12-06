---
layout: post
title: Golang 필드 유효성 검사 ozzo-validation 라이브러리 알아보기
---

오늘은 Golang으로 여러가지의 데이터 타입을 가지고 유효성 검사를 할 수 있는 ozzo-validation 라이브러리를 알아보려 합니다.

## ozzo-validation 설치

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

go get으로 ozzo-validation 패키지를 설치합니다.

```
go get github.com/go-ozzo/ozzo-validation
```

홈의 go 폴더에 ozzo-validation 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"regexp"

	validation "github.com/go-ozzo/ozzo-validation"
	"github.com/go-ozzo/ozzo-validation/is"
)
```

fmt, regexp 그리고 ozzo-validation을 가져옵니다.

```go
type ssh struct {
	Host     string
	User     string
	Port     string
	Password string
}
```

필드 검사에 사용할 구조체를 만듭니다.

```go
func (s ssh) Validate() error {
	return validation.ValidateStruct(&s,
		validation.Field(&s.Host, validation.Required),
		validation.Field(&s.Port),
		validation.Field(&s.User, validation.Required),
		validation.Field(&s.Password),
	)
}
```

Validate 메소드를 구현합니다.

구조체의 각자 필드에 대한 규칙을 지정합니다.

```go
func main() {
	value := "not_url"
	err := validation.Validate(value, validation.Required, is.URL)
	fmt.Println(err)
```

단순한 값을 검사합니다.

```go
	value = "not_digits"
	err = validation.Validate(value, validation.Match(regexp.MustCompile("^[0-9]$")).Error("must be a string with digits"))
	fmt.Println(err)
```

정규표현식에 맞지 않으면 특정 오류 메시지를 출력하게 할 수 있습니다.

```go
	s := ssh{
		Host: "127.0.0.1",
		Port: "8080",
	}
	err = s.Validate()
	fmt.Println(err)
```

먼저 만든 구조체로 객체를 만들어서 Validate 메소드로 검사합니다.

```go
	sliceValue := []ssh{
		ssh{Host: "127.0.0.1", Port: "8080"},
		ssh{Host: "127.0.0.1", User: "test", Password: "1234"},
		ssh{Host: "Unknown"},
	}
	err = validation.Validate(sliceValue)
	fmt.Println(err)
}
```

여러 객체를 슬라이스로 한 번에 검사합니다.