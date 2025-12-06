---
layout: post
title: Go 언어 Switch 대하여 배워보기
---

오늘은 Go 언어의 Switch문에 대하여 작성하면서 배워보려 합니다.

```go
package main

import (
	"fmt"
)

func main() {
	temp := 1
	switch temp {
	case 1:
		fmt.Println("1")
	case 2:
		fmt.Println("2")
	}
}
```

위와 같이 작성하면 switch문을 기본적으로 사용할 수 있습니다.

```go
package main

import (
	"fmt"
)

func main() {
	switch temp := 2 ; temp {
	case 1:
		fmt.Println("1")
	case 2:
		fmt.Println("2")
	}
}
```

간단한 수행문을 switch문으로 합쳐서 사용할 수 있습니다.

```go
package main

import (
	"fmt"
)

func main() {
	switch temp := 10 ; temp {
	case 1:
		fmt.Println("1")
	case 2:
		fmt.Println("2")
	default :
		fmt.Println("hello,world!")
	}
}
```

default 키워드를 사용하여 모든 케이스가 안 맞으면 출력되도록 합니다.

```go
package main

import (
	"fmt"
)

func main() {
	switch temp := 10 ; temp {
	case 0,10,20,30:
		fmt.Println("10?20?30?")
	default :
		fmt.Println("hello,world!")
	}
}
```

케이스를 여러개 중복하여 지정할 수 있습니다.

```go
package main

import (
	"fmt"
)

func main() {
	switch temp := 3.14; {
	case temp > 0 && temp < 10:
		fmt.Println("0~10")
	case temp == 3.14:
		fmt.Println("3.14")
	}
}
```

조건식을 케이스에 작성할 수 있습니다.

```go
package main

import (
	"fmt"
)

func main() {
	switch temp := 3.14; {
	case temp > 0 && temp < 10:
		fmt.Println("0~10")
		fallthrough
	case temp == 3.14:
		fmt.Println("3.14")
	}
}
```

fallthrough 키워드를 사용하여 다음 케이스로 물 흐르듯이 넘어가 케이스 검사를 수행합니다.