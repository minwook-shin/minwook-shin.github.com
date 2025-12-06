---
layout: post
title: Go 언어 채널 대하여 배워보기 
---

오늘은 Go 언어의 채널에 대하여 작성하면서 배워보려 합니다.

```go
package main

import (
	"fmt"
)

func main() {
	var a chan int
	fmt.Println(a)
	a = make(chan int)
	fmt.Println(a)
}
```

int 채널을 선언하기만 하면 nil으로 표기되고, make로 초기화하면 주소값이 할당됩니다.

```go
package main

import (
	"fmt"
)

func ch(c chan int)  {
	c <- 1
}

func main() {
	c := make(chan int)
	go ch(c)
	i := <- c
	fmt.Println(i)
}
```

고루틴 함수에서 채널에 송신하고, 메인 함수에서 변수로 수신하는 예시입니다.

채널은 고루틴 함수에서 데이터를 전송하기 전까지 기다리게 하기 때문에 이전처럼 시간을 이용하지 않습니다.

```go
package main

import (
	"fmt"
)

func ch2(c chan <- int)  {
	c <- 1
}

func main() {
	c2 := make(chan int)
	go ch2(c2)
	fmt.Println(<- c2)
}
```

송신만 전용으로 하게 채널 파라미터로 지정할 수 있습니다.

```go
package main

import (
	"fmt"
)

func ch(c chan <- int)  {
	c <- 1
}

func ch3(c2 <- chan int)  {
	i:=<-c2
	fmt.Println(i)
}

func main() {
	c3 := make(chan int)
	go ch(c3)
	go ch3(c3)
}
```

ch3 이라는 고루틴 함수의 태널 파라미터로 수신만 할수 있게 해보았습니다.

```go
package main

import (
	"fmt"
)

func ch4(c chan int) {  
    for i := 0; i < 5; i++ {
        c <- i
    }
    close(c)
}

func main() {
	c4 := make(chan int)
    go ch4(c4)
    for {
        v, o := <-c4
        if o == false {
            break
        }
        fmt.Println(v, ok)
    }
}
```

고루틴 함수에서 작업을 하다가 채널을 close로 닫으면 상태가 false로 변경되어 출력됩니다.

이를 이용하여 채널이 종료되는 것을 메인함수에서 감지할 수 있습니다.