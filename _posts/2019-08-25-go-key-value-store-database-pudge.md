---
layout: post
title: Golang 키와 값으로 파일 저장하는 데이터베이스 pudge 라이브러리 알아보기
---

오늘은 Golang으로 키와 값을 파일 혹은 인메모리로도 저장할 수 있는 pudge 라이브러리를 알아보려 합니다.

## pudge 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 pudge 패키지를 설치합니다.

```
go get github.com/recoilme/pudge
```

홈의 go 폴더에 pudge 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/recoilme/pudge"
)
```

fmt와 pudge를 가져옵니다.

```go
func main() {
	pudge.Set("test_path/test_file", "key", "value")
```

첫 인자로 주어진 파일 경로에 키와 값을 저장합니다.

```go
	variable := ""
	pudge.Get("test_path/test_file", "key", &variable)
	fmt.Println(variable)
```

첫 인자로 주어진 파일 경로에서 해당 키의 값을 가져옵니다.

```go
	pudge.DeleteFile("test_path/test_file")
```

주어진 파일 경로의 DB 파일을 지웁니다.

```go
	cfg := &pudge.Config{}
	db, err := pudge.Open("test_path/test_file", cfg)
	if err != nil {
		panic(err)
	}
```

FileMode 0666, DirMode 0777 으로 맞추어진 기본 설정을 가지고 DB를 생성합니다.

만약 StroreMode가 2로 설정되어있고, 파일 경로를 지정하지 않으면 인 메모리 데이터베이스로 사용할 수 있습니다.

```go
	type TestStruct struct {
		Fisrt  int
		Second int
	}
```

구조체를 만들어서 해당 형식에 맞게 값을 넣을 수 있습니다.

```go
	db.Set(1, &TestStruct{Fisrt: 1, Second: 1})
	db.Set(2, &TestStruct{Fisrt: 2, Second: 2})
```

저장할 때에 키와 인터페이스를 지원하므로 구조체 형식에 맞게 넣을 수도 있습니다.

```go
	var test TestStruct
	db.Get(1, &test)
	fmt.Println(test)
```

가져오는 것도 구조체 형식으로 가능합니다.

```go
	keys, _ := db.Keys(0, 2, 0, true)
	for _, key := range keys {
		var t TestStruct
		db.Get(key, &t)
		fmt.Println(t)
	}
```

0번부터 2개의 키를 가져올 수 있습니다.

```go
	defer db.DeleteFile()
```

DB 파일을 제거합니다.

```go
	if err := pudge.CloseAll(); err != nil {
		panic(err)
	}
}
```

마지막으로 열려있는 DB를 닫아줍니다.