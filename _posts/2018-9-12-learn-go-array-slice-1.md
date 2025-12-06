---
layout: post
title: Go 언어 배열과 슬라이스 대하여 배워보기 1 
---

오늘은 Go 언어의 배열과 슬라이스에 대하여 작성하면서 배워보려 합니다.

```go
package main

import (
	"fmt"
)

func main() {
	var arr[10] int
	fmt.Println(arr)
}
```

기본적으로 10개의 크기를 가진 배열을 만들려면 위와 같이 작성합니다.

```go
package main

import (
	"fmt"
)

func main() {
	var changeArr[10] int
	changeArr[0] = 1
	changeArr[9] = 1
	fmt.Println(changeArr)
}
```

만들어진 배열에 특정 인덱스를 지정하여 수를 변경할 수 있습니다.

```go
package main

import (
	"fmt"
)

func main() {
	var insertArr = [] int{1,2,3}
	fmt.Println(insertArr)
}
```

위와 같이 작성하면 배열보다 유연하게 만들 수 있는 슬라이스로 만들어집니다.

```go
package main

import (
	"fmt"
)
func main() {
	var resizeArr = [...]int{1,2,3}
	fmt.Println(resizeArr)
}
```

위와 같이 작성하면 컴파일러가 알아서 길이를 계산해줍니다.

```go
package main

import (
	"fmt"
)


func main() {
	var tmp = [3] int {1,2,3}
	var copyArr = tmp
	fmt.Println(copyArr)
}
```

배열을 변수에 대입하여 복사할 수 있습니다.

```go
package main

import (
	"fmt"
)

func printArr(temp[3]int){
	fmt.Println(temp)
}

func main() {
	var temp = [3] int {1,2,3}
	printArr(temp)
	fmt.Println(temp)
}
```

변수에 인자로 넣어서 사용할 수 있습니다.

```go
package main

import (
	"fmt"
)

func main() {
	var lengthTest = [] int{1,2,3}
    fmt.Println(len(lengthTest))
    var lengthTest2 = [...]int{1,2,3}
	fmt.Println(len(lengthTest2))
}
```

len으로 길이를 알수 있으며, 슬라이스를 사용할 때에 유용하게 사용됩니다.