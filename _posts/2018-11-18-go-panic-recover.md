---
layout: post
title: Go 언어 panic/recover 알아보기
---

오늘은 Go 언어에서의 panic과 recover를 간단한 코드로 알아보려 합니다.

```go
package main

import (
	"fmt"
)
```

우선 코드가 정상적으로 동작하는지 출력하여 확인하기 위해 fmt 패키지를 가져옵니다.

```go
func panicFunc()  {
	defer func() {
        if r := recover(); r != nil {
            fmt.Println(r)
		}
	}()
```

panic을 일으키는 코드가 들어있는 함수를 만들어주고, 패닉을 다시 돌려주는 recover로 해당 함수 이후 수행되는 코드를 실행하게 해줍니다.

defer 키워드로 해당 함수가 종료되기 전에 실행되어 아래의 코드를 수행한 후에 recover이 동작하게 됩니다.

```go
	arr := []int{1,2,3}
	fmt.Println(arr[3])
}
```

index out of range가 출력되는 오류 코드를 작성했습니다.

recover가 없다면 이 곳에서 코드 실행이 멈춥니다.

```go
func main() {
	// panic("panic!")
```

바로 오류를 띄우려면 위와 같이 panic을 수행하면 됩니다.

```go
	panicFunc()
	fmt.Println("recover!")
}
```

오류가 발생하는 panicFunc 함수에서 recover로 정상으로 회복되었기 때문에 다음 출력 문장이 실행되면서 프로그램은 종료합니다.