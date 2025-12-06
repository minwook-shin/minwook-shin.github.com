---
layout: post
title: Go 언어 반복문 대하여 배워보기
---

오늘은 Go 언어의 반복에 대하여 작성하면서 배워보려 합니다.

## for

```go
package main

import (
	"fmt"
)

func main() {
	count := 10
	for index := 0; index < count; index++ {
		fmt.Print(index)
	}
}
```

for문을 위와 같이 사용할 수 있습니다.

10을 count라는 변수에 assign하고 for문에서 count만큼 0부터 시작하는 index를 1씩 증가시키면서 출력하는 모습입니다.

## break

```go
package main

import (
	"fmt"
)

func main() {
	for index := 0; index < count; index++ {
		if index == 5 {
			break
		}
		fmt.Print(index)
	}
}
```

if로 index가 5일 때에 break 키워드를 써서 멈추게 하는 예제 코드입니다.

## continue

```go
package main

import (
	"fmt"
)

func main() {
	for index := 0; index < count; index++ {
		if index % 3 == 0 {
			continue
		}
		fmt.Print(index)
	}
}
```

continue 키워드를 사용하여 if로 index에 3을 나누었을 때 떨어지는 값이면 출력하지않고 지나가는 예제 코드입니다.

## ect

```go
package main

import (
	"fmt"
)

func main() {
	for ; count < 10 ; {
		
	}
	for count < 10 {
		
	}
}
```

세미콜론으로 된 구분으로 초기식과 증가식을 생략해도 되며, 그냥 작성해도 됩니다.

## infinite

```go
package main

import (
	"fmt"
)

func main() {
	for {
		fmt.Println("hello?")
	}
}
```

for 키워드만 사용하면 무한으로 반복할 수 있습니다.