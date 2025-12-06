---
layout: post
title: Go 언어 mutex 대하여 배워보기 
---

오늘은 Go 언어의 mutex에 대하여 작성하면서 배워보려 합니다.

```go
package main

import (  
	"time"
    "fmt"
	"sync"
)
```

우선 메인 함수가 끝나는 시간을 지연시키기 위한 time과 출력을 하기 위한 fmt, 그리고 mutex를 사용하기 위한 sync를 가져옵니다.

```go
func main() {
	var m sync.Mutex
	value := 0
```

Mutex를 생성해주고 테스트 값을 0으로 만들어줍니다.

```go
	go func() {
		for i := 0; i < 3; i++ {
			m.Lock()
			value = value + 10 
			fmt.Println(value)
			m.Unlock()
		}
	}()
```

고루틴 익명함수를 만들어서 3번 동안 mutex를 잠금과 잠금 해제를 반복하며 값을 더해줍니다.

```go
	go func() {
		for j := 0; j < 2; j++ {
			m.Lock()
			value = value - 10
			fmt.Println(value)
			m.Unlock()
		}
    }()
```

고루틴 익명함수를 만들어서 2번 동안 mutex를 잠금과 잠금 해제를 반복하며 값을 빼줍니다.

```go
	time.Sleep(1000000000)
}
```

메인 함수를 잠시 늦게 끝내면서 고루틴 익명 함수에서 결과를 출력하기를 기다립니다.

잠금이 걸리면 다른 고루틴에서 잠금을 풀 수 없기 때문에 순서대로 연산되고 출력되며 메인 함수가 종료됩니다.