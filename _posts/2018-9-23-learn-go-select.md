---
layout: post
title: Go 언어 select 대하여 배워보기 
---

오늘은 Go 언어의 select에 대하여 작성하면서 배워보려 합니다.

```go
package main

import (
	"time"
	"fmt"
)

func function(c chan int)  {
	time.Sleep(1)
	c <- 21
}

func main() {
	c :=make(chan int)
	go function(c)
	for {
		select {
			case value := <-c:
				fmt.Println(value)
				return
			default:
				fmt.Println("hello world")
			}
		}
}
```

고루틴 함수와 채널을 만들어서 계속 select문에서 신호를 받습니다.

hello world를 계속 출력하다가 고루틴 함수에서 sleep을 멈추고 신호를 보낼 때에 case에 채널이 걸려서 값이 출력되고 종료됩니다.

```go
package main

import (
	"time"
	"fmt"
)

func test1(c chan int)  {
	c <- 21
}
func test2(c chan int)  {
	c <- 12
}

func main() {
	c1 := make(chan int)
	c2 := make(chan int)
	go test1(c1)
	go test2(c2)
	
	time.Sleep(1)
	select{
	case v1 := <- c1:
		fmt.Println(v1)
	case v2 := <- c2:
		fmt.Println(v2)
	}
}
```

두 채널과 고루틴 함수를 만들어서 select문에 넣으면, 두 채널 중 먼저 도달한 값이 랜덤으로 출력됩니다. 