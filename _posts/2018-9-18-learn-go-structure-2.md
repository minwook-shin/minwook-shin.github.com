---
layout: post
title: Go 언어 구조체 대하여 배워보기 2
---

오늘도 어제에 이어서 Go 언어의 구조체에 대하여 작성하면서 배워보려 합니다.

```go
package main

import (
	"fmt"
)

func main() {
	type example5 struct {  
		firstName, lastName string
		age        int
	}
	e5 := &example5{"first","last",21}
	fmt.Println(e5,*e5,(*e5).age,e5.age)
}
```

포인터로 구조체를 가리킬 수 있습니다.

```go
package main

import (
	"fmt"
)

func main() {
	type example6 struct {  
		string
		int
	}
	e6 := example6{"first",21}
	fmt.Println(e6)
}
```

익명 필드로 타입만 작성할 수도 있습니다.

```go
package main

import (
	"fmt"
)

func main() {
	type nested struct{
		hello string
	}
	type structs struct{
		world nested
	}
	var e7 structs
	e7.world = nested{
		hello : "hello, wolrd!",
	}
	fmt.Println(e7)
}
```

구조체에 구조체를 넣을 수 있는 nested 형식의 구조체도 가능합니다.

```go
package main

import (
	"fmt"
)

func main() {
	type equals struct{
		text string
	}
	e8 := equals{
		text : "original",
	}
	e8Copy := equals{
		text : "copy",
	}
	fmt.Println(e8 == e8Copy)
}
```

구조체를 비교하여 필드가 같은지 확인할 수 있습니다.