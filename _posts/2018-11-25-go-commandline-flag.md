---
layout: post
title: Go 언어 Command line Flag 알아보기
---

오늘은 Go 언어에서 지원하는 커맨드라인 플래그를 간단한 코드로 알아보려 합니다.

```go
package main

import (
	"flag"
	"fmt"
)
```

커맨드라인 플래그를 파싱하는 패키지인 flag를 가져오고, 문자열을 출력할 fmt 패키지도 가져옵니다.

```go
func main() {
	wordPtr := flag.String("string", "default", "string")
	numPtr := flag.Int("int", 404, "int")
	boolPtr := flag.Bool("bool", true, "bool")
```

string, int, bool 타입으로 플래그를 선언할 수 있습니다.

flag.String,flag.Int,flag.Bool는 첫 인자로 포인터를 반환합니다.

```go
	var strVar string
	var intVar int
	var boolVar bool
	flag.StringVar(&strVar, "strVar", "default", "string")
	flag.IntVar(&intVar, "intVar", 404, "int")
	flag.BoolVar(&boolVar, "boolVar", true, "bool")
```

이미 선언된 변수를 사용하려면 해당 변수의 포인터를 전달합니다.

```go
	flag.Parse()
```

플래그를 파싱하기 위해 Parse 함수를 호출합니다.

```go
	fmt.Println(*wordPtr, *numPtr, *boolPtr)
	fmt.Println(strVar, intVar, boolVar)
	fmt.Println(flag.Args())
}
```

```(-int 99 -intVar 99  a1 aa a4)``` 와 같이 프로그램이 실행될 때에 같이 작성되어 파싱된 값을 출력합니다.

위와 같이 플래그에 값을 준 것은 주어진 값이 출력되고, 값을 주어지지 않은 것은 기본적으로 인자에 넣어둔 값이 출력됩니다.

또한 플래그에 해당되지 않는 인자들은 Args로 출력됩니다.