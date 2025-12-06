---
layout: post
title: Golang 유효성 검사 govalidator 라이브러리 알아보기
---

오늘은 Golang으로 문자열과 구조체를 유효성 검사할 수 있는 govalidator 라이브러리를 알아보려 합니다.

## govalidator 설치

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

go get으로 govalidator 패키지를 설치합니다.

```
go get github.com/asaskevich/govalidator
```

홈의 go 폴더에 govalidator 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/asaskevich/govalidator"
)
```

fmt와 govalidator를 가져옵니다.

```go
func init() {
	govalidator.SetFieldsRequiredByDefault(true)
}
```

반드시 존재해야되는 필드라는 명시를 하지 않아도 해당 옵션을 참으로 바꾸면 검사를 엄격하게 할 수 있습니다.

```go
type testStruct struct {
	Title string `valid:"-"`
	Email string `valid:"email"`
	URL   string `valid:"url"`
	IP    string `valid:"ip"`
}
```

유효성 검사에 사용될 구조체를 만듭니다.

```go
func main() {
	str := govalidator.ToString(&testStruct{Email: "test@example.com", IP: "127.0.0.1"})
	fmt.Println(str)
```

입력된 객체를 문자열로 바꿀 수 있습니다.

```go
	test := testStruct{Email: "test@example.com", IP: "127.0.0.1"}
	fmt.Println(test)

	result, err := govalidator.ValidateStruct(test)
	if err != nil {
		fmt.Println(err)
	}
	println(result)
```

필드에 붙은 태그를 기반으로 유효성 검사를 진행합니다.

위와 같이 부족한 필드는 Missing required field 라고 터미널에 출력됩니다.

```go
	println(govalidator.IsURL(`https://example.com`))
```

URL인지 검사할 수 있습니다.

```go
	intArray := []interface{}{1, 2, 3}
	var function govalidator.Iterator = func(v interface{}, _ int) {
		fmt.Println(v)
	}
	govalidator.Each(intArray, function)
```

슬라이스를 반복하여 Iterator를 호출합니다.

```go
	fmt.Println(govalidator.WhiteList("h1e2l3l4o5w6o7r8l9d0", "a-z"))
}
```

화이트리스트에 맞지 않는 문자들을 제거합니다.