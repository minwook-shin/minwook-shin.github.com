---
layout: post
title: Go 언어 ring 컨테이너 대하여 배워보기 
---

오늘은 Go 언어 컨테이너에 포함되있는 ring을 작성하면서 배워보려 합니다.

```go
package main

import (
	"container/ring"
	"fmt"
)
```

container/ring 패키지를 import 해줍니다.

```go
func main() {
    r := ring.New(3)
```

메인 함수에 ring을 생성해줍니다.

```go
	for i := 0; i < r.Len(); i++ {
		r.Value = i
		r = r.Next()
    }
```

Len으로 크기를 반환받아 for문으로 반복합니다.

반복하면서 값을 대입하고, 다음 값에 접근하기 위해 Next를 사용합니다.

```go
	r.Do(func(p interface{}) {
		fmt.Println(p.(int))
    })
```

각각의 원소에 실행할 익명함수를 등록하여 출력해줍니다.

```go
	s := ring.New(3)
	for j := 0; j < s.Len(); j++ {
		s.Value = 4
		s = s.Next()
	}
	rs := r.Link(s)
	rs.Do(func(p interface{}) {
		fmt.Println(p.(int))
	})
```

Link로 두 ring들을 서로 연결할 수 있습니다.

```go
	r = r.Move(3)
	r.Do(func(p interface{}) {
		fmt.Println(p.(int))
	})
}
```

Move로 ring을 회전시킬 수 있습니다.