---
layout: post
title: Go 언어 unicode/utf8 알아보기
---

오늘은 Go 언어 기본 패키지에 포함되있는 unicode/utf8을 이용하여 실습해보려 합니다.

utf8 인코딩으로 유니코드를 표현할 수 있습니다.

```go
package main

import (
	"fmt"
	"unicode/utf8"
)
```

unicode/utf8 패키지로 utf8로 인코딩과 디코딩을 하기 위해 가져옵니다.

fmt 패키지로 문자열을 출력하기 위해 가져옵니다.

```go
func main() {
	rInt := '한'
	byteBuffer := make([]byte, 3)
```

한글이 3바이트이기 때문에 바이트 배열로 3칸을 할당해줍니다.

```go
	n := utf8.EncodeRune(byteBuffer, rInt)
	fmt.Println(byteBuffer)
	fmt.Println(n)
```

할당한 바이트 배열에 값을 인코딩하여 넣으면 바이트 수가 반환됩니다.

```go
	r, size := utf8.DecodeRune(byteBuffer)
	fmt.Printf("%c %v\n", r, size)
```

디코드하여 문자로 다시 출력할 수 있습니다.


```go
	fmt.Println("bytes =", len(byteBuffer))
	fmt.Println("runes =", utf8.RuneCount(byteBuffer))
```

바이트의 크기랑 Rune의 크기를 출력할 수 있습니다.

```go
	fmt.Println(utf8.RuneLen('a'))
	fmt.Println(utf8.RuneLen('한'))
```

영단어와 한글의 바이트 크기를 비교할 수 있습니다.

```go
	valid := []byte("Hello, 고 언어")
	fmt.Println(utf8.Valid(valid))
	validRune := 'a'
	fmt.Println(utf8.ValidRune(validRune))
	validString := "Hello, 고"
	fmt.Println(utf8.ValidString(validString))
}
```

유효한 UTF-8 인코딩인지 판단해줍니다.

> 2018년 11월 8일 기준으로 일부 오타를 수정합니다.