---
layout: post
title: Golang JSON 인코딩 jsoniter 라이브러리 알아보기
---

오늘은 Golang으로 encoding/json 패키지를 대체할 수 있는 JSON 인코딩 jsoniter 라이브러리를 알아보려 합니다.

## jsoniter 설치

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

go get으로 jsoniter 패키지를 설치합니다.

```
go get github.com/json-iterator/go
```

홈의 go 폴더에 jsoniter 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"time"

	jsoniter "github.com/json-iterator/go"
)
```

fmt, time 그리고 jsoniter를 가져옵니다.

```go
func main() {
	type Book struct {
		Title string
		Score int
	}

	type Author struct {
		Name string
		Book
		Age     int `csv:"age,omitempty"`
		Created time.Time
	}
```

구조체를 만들어줍니다.

```go
	author := []Author{
		{
			Name:    "author_name_1",
			Book:    Book{"book_name_1", 80},
			Age:     22,
			Created: time.Now().UTC().Local(),
		},
		{
			Name: "author_name_2",
			Book: Book{"book_name_2", 70},
		},
	}
```

Author 객체를 생성합니다.

```go
	json := jsoniter.ConfigCompatibleWithStandardLibrary
	byteData, err := json.Marshal(&author)
	if err != nil {
		panic(err)
	}
```

jsoniter를 사용하여 데이터로 json 인코딩합니다.

```go
	fmt.Println(string(byteData))
```

인코딩된 바이트 데이터를 문자열로 출력합니다.

```go
	var authorCopy []Author
	err = json.Unmarshal(byteData, &authorCopy)
	if err != nil {
		panic(err)
	}
```

jsoniter를 사용하여 데이터로 json 디코딩합니다.

```go
	for _, authorData := range authorCopy {
		fmt.Println(authorData)
	}
}
```

디코딩된 Author 객체를 반복문으로 출력하여 내용을 확인합니다.