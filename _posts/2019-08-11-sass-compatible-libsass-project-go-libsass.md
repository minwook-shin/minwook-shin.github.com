---
layout: post
title: Golang Sass 3.5 컴파일러 go-libsass 라이브러리 알아보기
---

오늘은 Golang으로 Sass 컴파일러인 LibSass를 랩핑한 go-libsass 라이브러리를 알아보려 합니다.

## go-libsass 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 go-libsass 패키지를 설치합니다.

```
go get github.com/wellington/go-libsass
```

홈의 go 폴더에 go-libsass 소스코드와 패키지 파일이 생성됩니다.

## 샘플 scss

Sass 컴파일러에 사용할 Sass를 준비합니다.

```scss
@import "settings";

div {
    p { color: $bg;}
  }

div {
    p { color: black; }
}


div { 
    text: crypto(); 
}

div {}
```

import로 가져올 파일을 아래와 같이 작성합니다.

```scss
$bg: red
```

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go

import (
	"crypto/rand"
	"fmt"
	"os"

	"golang.org/x/net/context"

	libsass "github.com/wellington/go-libsass"
)
```

fmt, os, context, rand 그리고 go-libsass를 가져옵니다.

```go
func marshal(ctx context.Context, usv libsass.SassValue) (*libsass.SassValue, error) {
	b := make([]byte, 10)
	rand.Read(b)
	res, err := libsass.Marshal(fmt.Sprintf("'%x'", b))
	return &res, err
}
```

Sass 함수를 만들 수 있습니다.

```go
func main() {
	r, err := os.Open("main.scss")

	if err != nil {
		panic(err)
	}
```

main.scss를 가져옵니다.

```go
	libsass.RegisterSassFunc("crypto()", marshal)
```

이전에 만든 Sass 함수를 Sass 컴파일에서 사용할 수 있도록 등록합니다.

```go
	libsass.RegisterHeader(`
/*
 * RegisterHeader
 */`)
```

헤더를 추가하여 컴파일된 scss에서 나타낼 수 있습니다.

```go
	compiler, err := libsass.New(os.Stdout, r)

	if err != nil {
		panic(err)
	}
```

컴파일러 객체를 반환합니다.

```go
	path := []string{"config"}
	err = compiler.Option(libsass.IncludePaths(path))
	if err != nil {
		log.Fatal(err)
	}
```

config 폴더를 만들어서 해당 폴더의 scss를 찾을 수 있게 합니다.

해당 config 폴더에 _settings.scss 파일을 두고 메인 scss에서 import하면 됩니다.

```go
	if err := compiler.Run(); err != nil {
		panic(err)
	}
}
```

컴파일을 진행하고, os.Stdout에 의해 화면에 컴파일된 결과가 출력됩니다.