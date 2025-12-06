---
layout: post
title: Golang map 디코드 mapstructure 라이브러리 알아보기
---

오늘은 Golang으로 map을 디코드하여 일반적인 객체 값으로 나타나게 하는 mapstructure 라이브러리를 알아보려 합니다.

## mapstructure 설치

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

go get으로 mapstructure 패키지를 설치합니다.

```
go get github.com/mitchellh/mapstructure
```

홈의 go 폴더에 mapstructure 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"time"

	"github.com/mitchellh/mapstructure"
)
```

fmt, time 그리고 mapstructure를 가져옵니다.

```go
func main() {
	type Book struct {
		Title string
		Score int
	}

	type Author struct {
		Name    string
		Age     int
		Created time.Time
		Book
	}
```

구조체를 만들어줍니다.

```go
	author := map[string]interface{}{
		"Name":    "author_name_1",
		"Book":    Book{"book_name_1", 80},
		"Age":     "22",
		"Created": time.Now().UTC().Local(),
	}
```

map 인터페이스를 선언하고 데이터를 map에 담습니다.

```go
	var result Author
	config := &mapstructure.DecoderConfig{
		ErrorUnused:      true,
		ZeroFields:       true,
		WeaklyTypedInput: true,
		Result:           &result,
	}
```

사용되지 않은 키 오류 출력, 약한 변환 등의 디코드 설정을 작성합니다.

```go
	decoder, err := mapstructure.NewDecoder(config)
	if err != nil {
		panic(err)
	}
```

작성한 설정을 기반으로 디코더를 만듭니다.

```go
	err = decoder.Decode(author)
	if err != nil {
		panic(err)
	}
	fmt.Println(result)
}
```

미리 만들어둔 Author 객체에 map을 디코드하고 넣습니다.