---
layout: post
title: Go 언어 배열과 슬라이스 대하여 배워보기 2
---

오늘도 어제에 이어서 Go 언어의 배열과 슬라이스에 대하여 작성하면서 조금 더 배워보려 합니다.


```go
package main

import (
	"fmt"
)

func main() {
	var tmp2 = [] int {1,2,3}
	for index := 0; index < len(tmp2); index++ {
		fmt.Println(tmp2[index])
	}
}
```

슬라이스를 만들고 for문을 이용하여 슬라이스의 크기만큼 반복하며 해당 인덱스의 값을 출력합니다.

```go
package main

import (
	"fmt"
)

func main() {
	var multiArr [2][3]int
	fmt.Println(multiArr)	
}
```

2차원 배열도 만들 수 있습니다.

```go
package main

import (
	"fmt"
)

func main() {
	sliceArr := []int{1,2,3,4,5,6,7,8,9,10}
	fmt.Println(sliceArr[:],sliceArr[0:5],len(sliceArr[0:5]),cap(sliceArr[0:5]))
}
```

슬라이스의 length와 capacity를 구할 수 있습니다.

```go
package main

import (
	"fmt"
)

func main() {
	makeArr := make([]int, 3, 10)
	fmt.Println(makeArr)
}
```

make를 이용해서 length와 capacity를 지정한 슬라이스를 만들 수 있습니다.

```go
package main

import (
	"fmt"
)

func main() {
	realArr := []int{1,2,3}
	addArr := []int{1,2,3}
	fmt.Println(append(realArr,addArr...))
}
```

append를 이용하여 두개의 슬라이스를 붙일 수 있습니다 

```go
package main

import (
	"fmt"
)

func edit(temp[]int){
	for i,j := range temp {
		fmt.Println(i,j)
		temp[i] = 0
	}
}

func main() {
	editArr := []int{3,2,1}
	edit(editArr)
	fmt.Println(editArr)
}
```

함수에서 슬라이스를 고치면 함수가 종료되더라도 값은 저장됩니다.

```go
package main

import (
	"fmt"
)

func main() {
	copyArr2 := []int{1,2,3,4,5,6,7,8}
	tmpArr := make([]int,2)
	fmt.Println(copyArr2)
	fmt.Println(tmpArr)
	copy(tmpArr, copyArr2)
	fmt.Println(tmpArr)	
}
```

copy를 이용하여 슬라이스의 값을 또 다른 슬라이스에 옮길 수 있습니다.

만약 복사하려는 슬라이스의 크기보다 복사해놓을 슬라이스의 크기가 작다면 초과하는 길이는 잘린 채로 적용됩니다.