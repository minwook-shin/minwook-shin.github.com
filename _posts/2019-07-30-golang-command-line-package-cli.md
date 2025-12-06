---
layout: post
title: Golang 커맨드라인 패키지를 만들 수 있는 cli 라이브러리 알아보기
---

오늘은 Golang에서 커맨드라인 프로그램 패키지를 만들 수 있는 cli 라이브러리를 알아보려 합니다.

## cli 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 github에서 호스팅되고 있는 cli 패키지를 설치합니다.

```
go get github.com/mkideal/cli
```

홈의 go 폴더에 cli 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"os"

	"github.com/mkideal/cli"
)
```

fmt와 os 그리고 cli 패키지를 가져옵니다.

```go
type argT struct {
	cli.Helper

	Str    string         `cli:"*str" usage:"print string" dft:"hello"`
	I      int            `cli:"*i,integer" usage:"print integer" dft:"1"`
	B      bool           `cli:"*b" usage:"print boolean" dft:"True"`
	StrArr []string       `cli:"s,strArr" usage:"print string array`
	M      map[string]int `cli:"m" usage:"print map"`
```

문자열, 정수형, 논리, 슬라이스 타입의 인자를 받을 수 있으며, 별표로 꼭 받아야 되는 인자인지도 구별할 수 있습니다.

```go
	Version bool `cli:"!v" usage:"note the version"`
}
```

또한 느낌표는 강제로 Validate 과정을 통과하여 기존의 필수 인자를 무시할 수 있습니다.

```go
func (argv *argT) Validate(ctx *cli.Context) error {
	if argv.I > 10 {
		return fmt.Errorf("out of range")
	}
	if argv.B == false {
		return fmt.Errorf("invalid")
	}
	return nil
}
```

Validate 메소드로 들어오는 값을 검사할 수 있습니다.

```go
func main() {
	os.Exit(cli.Run(new(argT), func(ctx *cli.Context) error {
		argv := ctx.Argv().(*argT)
		if argv.Version {
			ctx.String("1.0\n")
			return nil
		}
		ctx.JSONln(ctx.Argv())
		return nil
	}))
}
```

메인 함수에서 값을 받아서 출력할 수 있습니다.