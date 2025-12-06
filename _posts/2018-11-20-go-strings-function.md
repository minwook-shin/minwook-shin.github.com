---
layout: post
title: Go 언어 string 함수 알아보기
---

오늘은 Go 언어에서 지원하는 string 함수를 간단한 코드로 알아보려 합니다.

```go
package main

import(
	"fmt"
	"strings"
)
```

출력하기 위한 fmt 패키지와 문자열을 조작할 strings 패키지를 가져옵니다.

```go
func main() {
	str := "hello, golang!"
```

테스트에 쓰일 문자열을 변수에 저장해줍니다.

```go
	fmt.Println(strings.Contains(str,"go"))
	fmt.Println(strings.ContainsRune(str,'G'))
```

문자열이 원본 문자열에 포함되어 있는지 알아볼 수 있습니다.

Rune 타입의 문자로도 해당 문자열에 포함되어 있는지 알아볼 수 있습니다.

```go
	fmt.Println(strings.Count(str,"l"))
```

해당 문자열이 몇번 반복하는지 알 수 있습니다.

```go
	fmt.Println(strings.HasPrefix(str,"hello"))
	fmt.Println(strings.HasPrefix(str,"golang!"))
```

문자열의 앞 부분에 해당 문자열이 있는지 판단할 수 있습니다.

```go
	fmt.Println(strings.HasSuffix(str,"hello"))
	fmt.Println(strings.HasSuffix(str,"golang!"))
```

문자열의 뒷 부분에 해당 문자열이 있는지 판단할 수 있습니다.

```go
	fmt.Println(strings.Index(str,"go"))
	fmt.Println(strings.IndexByte(str,'g'))
```

몇번째 위치에 해당 문자열과 문자가 있는지 알 수 있습니다.

```go
	fmt.Println(strings.Join([]string{"one","two"},"|"));
```

여러 문자열들을 합칠 수 있습니다.

```go
	fmt.Println(strings.Repeat(str, 3))
```

인자로 들어가는 숫자로 인해 문자열을 몇번 반복할지 정할 수 있습니다.

```go
	fmt.Println(strings.Replace(str,"o","0",1))
	fmt.Println(strings.Replace(str,"o","0",-1))
```

문자열의 특정 문자를 원하는 문자로 바꿔줄 수 있습니다.

```go
	fmt.Println(strings.Split("go,c++,c,java,python",","))
```

구분자를 지정하여 문자열을 나누어줄 수 있습니다.

```go
	fmt.Println(strings.ToLower("HELLO, GOLANG!"))
	fmt.Println(strings.ToUpper(str))
}
```

전부 소문자로 바꾸거나, 대문자로 변환할 수 있습니다.