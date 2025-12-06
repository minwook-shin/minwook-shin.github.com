---
layout: post
title: Go 언어 인터페이스 대하여 배워보기 
---

오늘은 Go 언어의 인터페이스에 대하여 작성하면서 배워보려 합니다.

```go
package main

import (
	"fmt"
)

type sample interface{
	generator() string
}

func main() {
	var s sample
	fmt.Println(s)
}
```

sample이라는 인터페이스에 generator라는 메소드를 모아두었습니다.

인터페이스가 비어있기 때문에 출력하면 nil이라고 출력됩니다.

```go
package main

import (
	"fmt"
)

type custom string

type sample interface{
	generator() string
}

func (c custom)generator()string{
	str := "hello world, "
	str += fmt.Sprintf("%v", c)
	return str
}

func main() {
    var s sample
	s = custom("go!")
	fmt.Println(s.generator())
}
```

sample 인터페이스에 있는 메소드를 구현하여 메인함수에서 사용한 모습니다.

```go
package main

import (
	"fmt"
)

func function(i interface{}) {  
    fmt.Printf(fmt.Sprintf("%v\n",i))
}

func main() {
	function("interface")
}
```

빈 인터페이스 타입으로 인자를 받으면 모든 타입을 받을 수 있습니다.

```go
package main

import (
	"fmt"
)

func function(i interface{}) {  
    fmt.Printf(fmt.Sprintf("%v\n",i))
}

func main() {
	t := struct {
        name string
    }{
        name: "name",
    }
	function(t)
}
```

익명 구조체 또한 빈 인터페이스 타입에 넣을 수 있습니다.