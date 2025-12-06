---
layout: post
title: Go 언어 GoLoutine 대하여 배워보기 
---

오늘은 Go 언어의 고루틴에 대하여 작성하면서 배워보려 합니다.

```go
package main

import (
	"time"
	"fmt"
)

func helloworld(){
	fmt.Println("hello, world!")
}

func main() {
	go helloworld()
	time.Sleep(1 * time.Second)
	fmt.Println("hello?")
}
```

메인 함수에서 go 키워드로 함수를 호출하면 런타임에서 고루틴을 실행시킵니다.

논리적 쓰레드로서 비동기적으로 함수를 실행하며, 여러 코드를 동시에 실행시킵니다.

메인 함수가 끝나면 고루틴으로 실행된 함수가 출력을 하기도 전에 종료되므로, 강제로 기다려줘야 합니다.

```go
package main

import (
	"time"
	"fmt"
)

func say1() {  
    for i := 0; i < 10; i++ {
		fmt.Println("hello!")
		time.Sleep(1 * time.Millisecond)
	}
}

func say2() {  
    for i := 0; i < 10; i++ {
		fmt.Println("go?")
		time.Sleep(1* time.Millisecond)
	}
}

func main() {
	go say1()
	go say2()
	time.Sleep(100 * time.Millisecond)
}
```

여러 함수를 고루틴으로 실행하면 기존처럼 순서대로 나오지 않고, 랜덤으로 서로 섞여서 나옵니다.

이 역시 sleep으로 메인에서 기다려줘야 합니다.

```go
package main

import (
	"time"
	"fmt"
)

func main() {
	go func() {
		for i := 0; i < 10; i++ {
			fmt.Println("hello! go function!")
			time.Sleep(1 * time.Millisecond)
		}
	}()
	time.Sleep(10 * time.Millisecond)
}
```

익명함수로 고루틴을 실행할 수 있습니다.

```go
package main

import (
	"sync"
	"time"
	"fmt"
)

func main() {
	var w sync.WaitGroup
	w.Add(2)
	go func() {
		defer w.Done()
		for i := 0; i < 2; i++ {
		fmt.Println("hello!")
		}
	}()
	go func() {
		defer w.Done()
		for i := 0; i < 2; i++ {
		fmt.Println("go function!")
		}
	}()
	w.Wait()
}
```

언제까지나 sleep 시킬 수는 없기 때문에, WaitGroup를 만들어서 2개의 고루틴 함수를 기다려주게 할 수 있습니다.

각 고루틴 함수에서 Done을 실행시키게 합니다.

defer 키워드는 타 언어의 finally 처럼 함수가 리턴되기 전에 실행되게 할 수 있습니다.

마지막으로 메인함수에서 wait 로 메인 함수의 마지막까지 코드가 실행되도 종료되지 않도록 해줍니다.