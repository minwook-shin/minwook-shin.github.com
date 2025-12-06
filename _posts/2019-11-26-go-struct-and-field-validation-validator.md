---
layout: post
title: Golang 구조체 필드 검사 validator 라이브러리 알아보기
---

오늘은 Golang으로 태그 기반의 구조체 필드 유효성 검사하는 validator 라이브러리를 알아보려 합니다.

## validator 설치

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

go get으로 validator 패키지를 설치합니다.

```
go get github.com/go-playground/validator
```

홈의 go 폴더에 validator 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/go-playground/validator"
)
```

fmt와 validator를 가져옵니다.

```go
type Account struct {
	ID       string `validate:"required"`
	Password string `validate:"required"`
	Email    string `validate:"required,email"`
	Age      uint8  `validate:"gte=0,lte=100"`
}
```

검사할 구조체를 만듭니다.

```go
func main() {
	validate := validator.New()
```

validate 객체를 만듭니다.

```go
	account := &Account{
		ID:       "test_id",
		Email:    "test@example.com",
		Age:      20,
	}
```

위 required 태그가 달려있는 필드를 빼고 객체로 만듭니다.

```go
	err := validate.Struct(account)
	if err != nil {
		for _, err := range err.(validator.ValidationErrors) {
			fmt.Println(err)
		}
		return
	}
```

required 태그가 있는 필드가 없으면 아래와 같이 출력됩니다.

Field validation for 'Password' failed on the 'required' tag

```go
	email := "test@example.com"
	err = validate.Var(email, "required,email")
	if err != nil {
		for _, err := range err.(validator.ValidationErrors) {
			fmt.Println(err)
		}
		return
	}
}
```

단일 변수도 태그로 검사할 수 있습니다.