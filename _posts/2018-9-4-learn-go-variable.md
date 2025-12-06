---
layout: post
title: Go 언어 변수에 대하여 배워보기
---

오늘은 Go 언어의 변수에 대하여 작성하면서 배워보려 합니다.

## 기본

```go
package main

import (
	"fmt"
)

func main() {
	var count int
	fmt.Println(count)
	count = 1
	fmt.Println(count)
}
```

var 키워드로 int 타입의 변수를 선언해주고, 값을 대입할 수 있습니다.

값을 넣지 않으면 스스로 0으로 초기화됩니다.

## 추론

```go
package main

import (
	"fmt"
)

func main() {
	// var tmp int = 10
	// fmt.Println(tmp)
	var tmp2 = 10
	fmt.Println(tmp2)
}
```

주석에서 작성한것처럼 타입을 명시하고 값을 대입해도 되지만, 타입을 추론할 수 있습니다.

## 복수 변수

```go
package main

import (
	"fmt"
)

func main() {
	var age,time int = 21, 1000
	fmt.Println(age,time)

	var age2,time2 = 21, 1000
	fmt.Println(age2,time2)
}
```

여러개의 변수들에 순서대로 값을 넣어줄 수 있습니다.

## var

```go
package main

import (
	"fmt"
)

func main() {
	var(c1,c2 int = 10,10)
	fmt.Println(c1,c2)
}
```

var 키워드로 묶어서 변수를 선언하고 초기화할 수 있습니다.

## 축약형

```go
package main

import (
	"fmt"
)

func main() {
	name := "name"
	fmt.Println(name)
}
```

축약형을 이용하여 간단하게 변수를 만들 수 있습니다.

## 축약형 복수 변수

```go
package main

import (
	"fmt"
)

func main() {
	name := "name"
	name,hello := "world" , "hello"
	fmt.Println(name, hello)
}
```

같은 변수 이름을 또 시도하면 오류가 생기지만, 새로운 변수를 추가해서 같이 하면 기존에 있던 변수의 값도 변경되면서 반영됩니다.