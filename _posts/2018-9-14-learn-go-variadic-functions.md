---
layout: post
title: Go 언어 가변인자 함수 대하여 배워보기
---

오늘은 Go 언어의 가변인자 함수에 대하여 작성하면서 배워보려 합니다.

```go
package main

import (
	"reflect"
	"fmt"
)

func testInt(n ... int)  {
	fmt.Println(reflect.TypeOf(n),n)
}

func main() {
	testInt(1,2,3)
	testInt(1,2,3,4,5)
}
```

가변인자 함수를 위와 같이 작성하여 사용할 수 있습니다.

가변인자로 받은 것은 슬라이스로 타입이 표기됩니다.

```go
package main

import (
	"reflect"
	"fmt"
)

func testArray(n ... []int)  {
	fmt.Println(reflect.TypeOf(n),n)
}

func main() {
	var a = make([]int,5,10)
	testArray(a)
	var b = make([]int,10,10)
	testArray(b)
}
```

슬라이스를 가변인자 함수에 넣으면 2차원 슬라이스로 됩니다.

슬라이스를 넣을 때는 ```[]```를 꼭 잊지 말아야 합니다.

```go
package main

import (
	"fmt"
)

func changeInt1(n ... []int){
	n[0][0] = 1
}

func main() {
	c := make([]int,3,3)
	changeInt1(c)
	fmt.Println(c)
}
```

일반적으로 함수에 배열을 넣어서 수정했을 때처럼 main에도 수정된 값이 반영됩니다.

다만 슬라이스를 넣으면 2차원 배열처럼 접근해야 됩니다.

```go
package main

import (
	"fmt"
)

func changeInt2(n ... int){
	n[0] = 1
}

func main() {
	d := make([]int,3,3)
	changeInt2(d...)
	fmt.Println(d)
}
```

그래서 위와 같이 코드를 변경해서 작성하면 가변인자를 int형으로 받기 때문에 평소에 사용하던 차원대로 수정할 수 있습니다.