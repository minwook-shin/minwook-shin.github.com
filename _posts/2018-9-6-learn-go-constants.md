---
layout: post
title: Go 언어 상수에 대하여 배워보기
---

오늘은 Go 언어의 상수에 대하여 작성하면서 배워보려 합니다.

## const

```go
package main

import (
	"fmt"
)

func main() {
	const a = 10
	fmt.Println(a)
}
```

const 키워드를 이용하여 상수를 만들 수 있습니다.

## function

```go
package main

import (
	"math"
	"fmt"
)

func main() {
	const pi = math.Pi
	fmt.Println(pi)
	// const sqrt = math.Sqrt(1)
}
```

함수의 반환 값은 const로 지정할 수 없습니다.

## string

```go
package main

import (
	"fmt"
)

func main() {
	const hello = "Hello"
	fmt.Println(hello)
}
```

문자열도 상수로 지정할 수 있습니다.

## custom type

```go
package main

import (
	"fmt"
)

func main() {
	type str string
	var world str = "world"
	// world = string("")
	fmt.Println(world)
}
```

type 키워드는 새로운 타입임을 알려주며 구조체를 만들 때에 사용되지만, 위와 같이 커스터마이즈할 수 있습니다.

단, 새로운 타입을 만들었으면 string으로 바로 넣을 수는 없습니다.

## boolean

```go
package main

import (
	"fmt"
)

func main() {
	const boolean = true
	fmt.Println(boolean)
}
```

bool 타입인 true/false를 상수에 넣을 수 있습니다.

## assign

```go
package main

import (
	"fmt"
)

func main() {
	const c = 3
    v := c
    var v32 int32 = c
    var v64 float64 = c
    var c64 complex64 = c
    fmt.Println(v, v32, v64, c64)
}
```

위 예제 코드에서 c는 타입이 명시되지 않은 3인 상수이기 때문에 assign하는 순간 타입이 결정됩니다.

