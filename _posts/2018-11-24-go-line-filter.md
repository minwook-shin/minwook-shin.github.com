---
layout: post
title: Go 언어 line filter 알아보기
---

오늘은 Go 언어에서 지원하는 라인 필터를 간단한 코드로 알아보려 합니다.

stdin을 통해 입력을 받은 뒤에 stdout으로 출력하는 프로그램을 라인 필터라고 합니다.

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strings"
)
```

스캐너를 만들고, stdin으로 입력을 받기 위해 bufio와 os 패키지를 가져옵니다.

문자열을 조작하기 위해서 strings 패키지를 가져오고, 출력을 위해 fmt 패키지도 가져옵니다.

```go
func main() {
	scanner := bufio.NewScanner(os.Stdin)
```

스캐너로 stdin을 받습니다.

```go
	for scanner.Scan() {
```

입력을 라인 단위로 나누어 for문을 반복합니다.

```go
		if scanner.Text() == "!q" {
			os.Exit(0)
		}
```

만약 특정 문자열이 들어오면 프로그램을 종료하도록 작성합니다.

```go
		fmt.Println(strings.Contains(scanner.Text(), "hello"))
		fmt.Println(strings.Count(scanner.Text(), "l"))
		fmt.Println(strings.Index(scanner.Text(), "hello"))
		fmt.Println(strings.Repeat(scanner.Text(), 3))
		fmt.Println(strings.Split(scanner.Text(), ","))
		fmt.Println(strings.ToUpper(scanner.Text()))
	}
```

string 함수들을 사용하여 스캐너로 입력받은 문자열을 조작해줄 수 있습니다.

```go
	if err := scanner.Err(); err != nil {
		os.Exit(1)
	}
}
```

스캐너로 읽으면서 오류를 판별해줍니다.