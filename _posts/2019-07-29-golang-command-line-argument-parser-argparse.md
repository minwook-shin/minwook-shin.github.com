---
layout: post
title: Golang 커맨드라인 인자 파싱하는 argparse 라이브러리 알아보기
---

오늘은 Golang에서 커맨드라인에서 프로그램의 인자를 파싱해서 값을 가져오는 argparse 라이브러리를 알아보려 합니다.

## argparse 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 github에서 호스팅되고 있는 argparse 패키지를 설치합니다.

```
go get -u -v github.com/akamensky/argparse
```

홈의 go 폴더에 argparse 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"os"

	"github.com/akamensky/argparse"
)
```

fmt와 os, 그리고 argparse 패키지를 가져옵니다.

```go
func main() {
	parser := argparse.NewParser("test_program", "test argparse program")
```

main 함수에 argparse의 NewParser로 새로운 Parser 객체를 만듭니다.

```go
    s := parser.String("v", "value", &argparse.Options{Required: true, Help: "Value to print"})
```

인자로 String을 받을 수 있습니다.

이 때에 필수로 받아야 될 지 정하는 것과 도움말과 같은 옵션을 적을 수 있습니다.

```go
    f := parser.Flag("b", "bool", &argparse.Options{Required: true, Help: "Bool to print"})
```

인자로 Bool을 받을 수 있습니다.

이 때에 필수로 받아야 될 지 정하는 것과 도움말과 같은 옵션을 적을 수 있습니다.

```go
    l := parser.List("l", "list", &argparse.Options{Required: true, Help: "List to print"})
```

인자로 List를 받을 수 있습니다.

이 때에 필수로 받아야 될 지 정하는 것과 도움말과 같은 옵션을 적을 수 있습니다.

```go
    S := parser.Selector("s", "seletor", []string{"PRODUCT", "TEST"}, &argparse.Options{Required: true, Help: "Select to print"})
```

인자로 Selector를 받을 수 있습니다.

이 때에 필수로 받아야 될 지 정하는 것과 도움말을 적을 수 있습니다.

```go
	F := parser.File("f", "file", os.O_RDWR, 0600, &argparse.Options{Required: true, Help: "File to print"})
```

인자로 File을 받을 수 있습니다.

이 때에 필수로 받아야 될 지 정하는 것과 도움말과 같은 옵션을 적을 수 있습니다.

```go
	err := parser.Parse(os.Args)
	if err != nil {
		fmt.Print(parser.Usage(err))
    }
```

Parser 객체에 인자를 가져와서 파싱합니다.

```go
	fmt.Println(*s, *f, *l, *S, *F)
}
```

마지막으로 파싱한 내용을 출력하여 확인할 수 있습니다.