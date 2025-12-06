---
layout: post
title: Go 언어 Map 대하여 배워보기
---

오늘은 Go 언어의 Map에 대하여 작성하면서 배워보려 합니다.

```go
package main

import (
	"fmt"
)

func main() {
	var a = make(map[string]int)
	a["key1"] = 1
	a["key2"] = 2
	fmt.Println(len(a),a)
}
```

이전에 슬라이스를 make로 만든 것처럼 map도 make로 만들어줘야 합니다.

처음부터 ```var a map[string]int```로 작성했다가는 nil이 없으므로 panic이 나타납니다.

위와 같이 작성하면 총 두 쌍의 키와 값이 생깁니다.

map도 길이를 len으로 측정할 수 있습니다.

```go
package main

import (
	"fmt"
)

func main() {
	var b = map[string]int{
		"key1" : 1,
		"key2" : 2,
	}
	fmt.Println(b)
}
```

map을 만들면서 초기화하면 make를 안 쓰고도 만들 수도 있습니다.

```go
package main

import (
	"fmt"
)

func main() {
	var db = map[string]int{
		"key1" : 1,
		"key2" : 2,
	}
	fmt.Println(db["key1"])
	fmt.Println(db["key3"])
	v1 , status := db["key1"]
	fmt.Println(v1, status)
	v2 , status := db["key3"]
	fmt.Println(v2, status)
}
```

키를 검색하면 값이 출력되는데, 없는 키를 입력하면 0이 출력됩니다.

또한 받는 변수가 두개면 순서대로 값, 존재 유무가 출력됩니다.

```go
package main

import (
	"fmt"
)

func main() {
	forMap := map[string]int{
		"key1" : 1,
		"key2" : 2,
	}
	for i,j := range forMap{
		fmt.Println(i,j)
	}
}
```

for문을 이용하여 키와 값을 순서대로 출력할 수 있습니다.

```go
package main

import (
	"fmt"
)

func main() {
	var findMap = map[string]int{
		"key1" : 1,
		"key2" : 2,
	}
	fmt.Println(findMap["key1"])
	delete(findMap,"key1")
	fmt.Println(findMap["key1"])
}
```

값을 삭제하고 싶을 때에는 delete로 삭제하고 싶은 map과 해당 키를 넣어주면 삭제할 수 있습니다.