---
layout: post
title: Go 언어 조건문 대하여 배워보기
---

오늘은 Go 언어의 조건문에 대하여 작성하면서 배워보려 합니다.

## if else

```go
package main

import (
	"fmt"
)

func main() {
	example := "hello, world!"
	if example == "hello, world!" {
		fmt.Println("hello")
	} else{
		fmt.Println("hello")
	}
}
```

기본적으로 조건문은 위와 같이 사용합니다.

## statement

```go
package main

import (
	"fmt"
)

func main() {
	if temp := 3.14; temp > 3 {
		fmt.Println("true!")
	}else{
		fmt.Println("false")
	}
}
```

짧은 수행문을 if에 포함할 수 있습니다.

조건문과는 세미클론으로 구별합니다.

## else if

```go
package main

import (
	"fmt"
)

func main() {
	if temp2 := 3.14; temp2 < 3 {
		fmt.Println("true!")
	}else if temp2 == 3.14{
		fmt.Println("pi!")
	}else{
		fmt.Println("false")
	}
}
```

else if는 위와 같이 사용합니다.