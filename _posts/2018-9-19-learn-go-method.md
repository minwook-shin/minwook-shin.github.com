---
layout: post
title: Go 언어 메소드 대하여 배워보기 
---

오늘은 Go 언어의 메소드에 대하여 작성하면서 배워보려 합니다.

```go
package main

import (
	"fmt"
)

type sample struct{
	str string
	i int
}

func (e sample)method()  {
	fmt.Println(e.str,e.i)
}

func main() {
	example := sample{
		"hello, world!",
		10,
	}
	example.method()
}
```

구조체를 생성하고 그 구조체를 위한 메소드를 만들어서 메인 함수에서 구조체의 메소드를 호출할 수 있습니다.

```go
package main

import (
	"fmt"
)

type sample struct{
	str string
	i int
}

func method2(e sample)  {
	fmt.Println(e.str,e.i)
}

func main() {
	example2 := sample{
		"hello, world!",
		10,
	}
	method2(example2)
}
```

인자로 구조체를 넣을 수도 있습니다.

```go
package main

import (
	"fmt"
)

type mathPI struct{
	pi float32
}

func (m mathPI)method3() float32{
	return m.pi
}

func main() {
	m := mathPI{
		3.14,
	}
	fmt.Println(m.method3())
}
```

메소드에서 값을 반환하여 출력할 수 있습니다.

```go
package main

import (
	"fmt"
)

type ptr struct{
	i int
}
func (p *ptr)method4(new int){
	p.i = new
}

func main() {
	p := ptr{
		i : 10,
	}
	fmt.Println(p.i)
	(&p).method4(20)
    fmt.Println(p.i)
    
	fmt.Println(p.i)
	p.method4(30)
	fmt.Println(p.i)
}
```

포인터 리시버로 구조체의 포인터를 전달하여 메소드에서 값이 변경되면 그대로 값이 적용됩니다.

굳이 &를 사용하지 않아도 동일한 결과를 가져옵니다.