---
layout: post
title: Golang TOML 포맷 파싱 toml 라이브러리 알아보기
---

오늘은 Golang으로 TOML 포맷의 파일을 파싱하고 인코딩할 수 있는 toml 라이브러리를 알아보려 합니다.

## toml 설치

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

go get으로 toml 패키지를 설치합니다.

```
go get github.com/BurntSushi/toml
```

홈의 go 폴더에 toml 소스코드와 패키지 파일이 생성됩니다.

## 예제

```
title = "TOML Example"

[Entity]
name = "test_name"
password = "0000"
description = "hello world!"
testTIme = 2019-11-04T00:00:00Z
```

우선 파싱할 test.toml 파일을 만들어봅니다.

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"time"

	"github.com/BurntSushi/toml"
)
```

fmt, time 그리고 toml을 가져옵니다.

```go
type testStruct struct {
	Title  string
	Entity entityInfo
}

type entityInfo struct {
	Name        string `toml:"name"`
	Password    string `toml:"password"`
	Description string `toml:"description"`
	TestTIme    time.Time
}
```

testStruct와 entityInfo라는 구조체를 만듭니다.

해당 구조체는 파싱할 test.toml 파일의 구조를 따라갑니다.

```go
func main() {
	var test testStruct
	_, err := toml.DecodeFile("test.toml", &test)
	if err != nil {
		panic(err)
	}
```

testStruct 객체를 만들어서 test.toml 파일을 파싱처럼 디코드 합니다.

```go
	fmt.Println(test.Title)
	fmt.Println(test.Entity.Name)
	fmt.Println(test.Entity.Password)
	fmt.Println(test.Entity.Description)
	fmt.Println(test.Entity.TestTIme)
}
```

testStruct 객체에 test.toml 파일 내용을 담아놨으므로 출력해볼 수 있습니다.