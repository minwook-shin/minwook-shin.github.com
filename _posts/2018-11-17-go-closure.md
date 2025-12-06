---
layout: post
title: Go 언어 클로저 알아보기
---

오늘은 Go 언어에서 지원하는 클로저를 간단한 코드로 알아보려 합니다.

클로저란 함수 밖에 존재하는 변수를 접근하거나 수정하는 함수 값이라고 하며, 변수에 익명함수를 반환하면서 사용합니다.

```go
package main

import "fmt"
```

우선 잘 반환되었는지 출력하여 확인하기 위해 fmt 패키지를 가져옵니다.

```go
func insertNum() func(n int) int {
    total := 0
```

평범한 함수처럼 생겼지만, 반환 타입이 정수를 반환하는 함수로 지정되어 있습니다.

그리고 클로저에 쓰일 변수도 초기화해줍니다.

```go
    return func(n int) int {
        total = total + n
        return total
    }
}
```

이제 익명 함수를 반환해보겠습니다.

total이라는 익명 함수의 외부 변수를 사용하는 것을 보실 수 있습니다.

```go
func main() {
    adder := insertNum()
```

adder에 func(n int) int 타입이 부여됨을 확인할 수 있습니다.

```go
    fmt.Println(adder(4))
    fmt.Println(adder(400))
}
```

익명 함수가 반환되면서 외부의 변수에 누적되면 최종적으로는 404가 출력됩니다.