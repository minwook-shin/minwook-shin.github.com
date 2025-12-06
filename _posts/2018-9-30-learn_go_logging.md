---
layout: post
title: Go 언어 logging 대하여 배워보기 
---

오늘은 Go 언어에서 간단하게 로그를 남겨보는 법을 작성하면서 배워보려 합니다.

```go
package main
 
import (
    "log"
    "os"
)

func test() {
    log.Print("Test")
}
  
func main() {
	test()
}
```

기본적으로 날짜와 시간이 같이 로그로 출력됩니다.

```go
package main
 
import (
    "log"
    "os"
)
 
var logging *log.Logger
 
func main() {
    logging = log.New(os.Stdout, "prefix: ", log.LstdFlags)
	logging.Println("include info")
}
```

Logger 타입으로 새로운 log를 만들기 위해서는 New 함수를 사용합니다.

```go
package main
 
import (
    "log"
    "os"
)
 
var logging *log.Logger
 
func main() {
    logging = log.New(os.Stdout, "prefix: ", log.LstdFlags)
	logging.SetFlags(0)
	logging.Println("remove time")
}
```

로그에 날짜와 시간이 없애고 싶다면 SetFlags 함수에 0을 지정하면 됩니다.

```go
package main
 
import (
    "log"
    "os"
)

var logging *log.Logger
 
func main() {
    logging = log.New(os.Stdout, "", log.LstdFlags)
	logging.SetPrefix("prefix: ")
	logging.Println("set prefix")
}
```

SetFlags 함수처럼 SetPrefix 함수로도 따로 Prefix를 설정할 수 있습니다.