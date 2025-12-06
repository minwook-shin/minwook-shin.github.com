---
layout: post
title: Go 언어 포인터 대하여 배워보기
---

오늘은 Go 언어의 포인터에 대하여 작성하면서 배워보려 합니다.

```go
package main

import (
	"reflect"
	"fmt"
)

func main() {
	a := 10
	b :=  &a
	fmt.Println(a)
	fmt.Println(reflect.TypeOf(b),b)
}
```

위 코드는 10이라는 변수의 주소를 포인터에 저장하여 출력하는 예제입니다.

```go
package main

import (
	"fmt"
)

func main() {
	var nilPtr *int
	fmt.Println(nilPtr)
}
```

포인터만 준비하고 아직 초기화는 안된 상태로 만들어 둘 수 있습니다.

```go
package main

import (
	"fmt"
)

func main() {
	v := 10
	ptr := &v
	fmt.Println(*ptr)
}
```

포인터가 가리키는 값을 나타낼 때는 위와 같이 나타냅니다.

```go
package main

import (
	"fmt"
)

func main() {
	v2 := 10
	ptr2 := &v2
	*ptr2++
	fmt.Println(*ptr2)
}
```

포인터가 가리키는 변수의 값을 증가시키려면 ++를 이용합니다.

```go
package main

import (
	"fmt"
)

func main() {
	v3 := 10
	ptr3 := &v3
	*ptr3 = 20
	fmt.Println(*ptr3)
}
```

포인터가 가리키는 값에 대입할 수 있습니다.

```go
package main

import (
	"reflect"
	"fmt"
)

func main() {
	arr := [3]int{1,2,3}
	arrPtr := &arr
	fmt.Println(arrPtr)
}
```

배열을 포인터로 가리킬 수 있습니다.