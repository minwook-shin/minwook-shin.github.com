---
layout: post
title: Go 언어 구조체 대하여 배워보기 1
---

오늘은 Go 언어의 구조체에 대하여 작성하면서 배워보려 합니다.

```go
package main

import (
	"fmt"
)

func main() {
	type example1 struct{
		name string
		age int
	}
	e1 := example1{
		name : "name",
		age : 21,
	}
	fmt.Println(e1)
}
```

기본적으로 위와 같이 작성하면 go언어에서 구조체를 만들어 사용할 수 있습니다.

예제 구조체를 만들고 string과 int 구조체 필드를 만들어주었습니다.

그리고 변수에 대입하면서 필드를 초기화해줍니다.

```go
package main

import (
	"fmt"
)

func main() {
	type example2 struct{
		firstName, lastName string
		age int
	}
	e2 := example2{
		firstName : "first",
		lastName : "last",
		age: 21,
	}
	fmt.Println(e2)
}
```

구조체 필드에 같은 타입이면 한 줄에 쓸 수 있습니다.

```go
package main

import (
	"fmt"
)

func main() {
	e3 := struct{
		name string
		age int
		}{
		name :"name",
		age : 21,
	}
	fmt.Println(e3)
}
```

익명 구조체를 만들어서 변수에 바로 대입할 수 있습니다.

선언과 초기화를 하면서 바로 변수에 넣는 예시는 위와 같습니다.

```go
package main

import (
	"fmt"
)

func main() {
	e4 := struct{
		name string
		age int
		}{
		name :"name",
		age : 21,
	}
	fmt.Println(e4.name,e4.age)
}
```

구조체를 변수에 대입한 뒤에 구조체에 접근하여 값을 읽을 수 있습니다.