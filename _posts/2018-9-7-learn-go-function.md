---
layout: post
title: Go 언어 함수에 대하여 배워보기
---

오늘은 Go 언어의 함수에 대하여 작성하면서 배워보려 합니다.

## 기초

```go
package main

import (
	"fmt"
)

func helloworld(){}

func main() {
	helloworld()
}
```

함수의 기본적인 틀은 위와 같이 작성하면 됩니다.

```go
package main

import (
	"fmt"
)

func printValue(){
	fmt.Println(3.14)
}

func main() {
	printValue()
}
```

만약 내용을 넣는다면 위와같이 작성됩니다.

## 인자

```go
package main

import (
	"fmt"
)
func sum(a,b int){
	fmt.Println(a + b)
}
func main() {
	sum(1,2)
}
```

인자를 넣을 수 있습니다.

## 반환 값

```go
package main

import (
	"fmt"
)

func returnValue()float64{
	return 3.14
}

func main() {
	fmt.Println(returnValue())
}
```

```go
package main

import (
	"fmt"
)
func returnSum(a,b int)int{
	return a + b
}

func main() {
	fmt.Println(returnSum(1,2))
}
```

함수에 return 키워드로 반환하고 싶은 값을 쓰고, 함수가 종료될 때에 값이 반환됩니다.

## 다중 반환

```go
package main

import (
	"fmt"
)
func returnTwoValue(age,count int)(int,int){
	value1 := age
	value2 := count + 1 
	return value1, value2
}

func main() {
	fmt.Println(returnTwoValue(20,0))
}
```

함수가 종료되었을 때 반환할 값을 여러개 지정할 수 있습니다.

## Named 반환 인자

go 언어에서는 Named Return Parameter 라는 기능이 있습니다.

반환할 변수를 미리 반환 타입을 기입하는 곳에 작성해줍니다.

```go
package main

import (
	"fmt"
)
func named(a,b int)(r1 , r2 int){
	r1 = a + 1
	r2 = b + 1
	return
}

func main() {
	fmt.Println(named(1,2))
}
```

미리 변수를 만들어두었기 때문에 마지막에 return 키워드만 사용해주면 됩니다.

## 빈 식별자

```go
package main

import (
	"fmt"
)

func returnTwoValue(age,count int)(int,int){
	value1 := age
	value2 := count + 1 
	return value1, value2
}

func main() {
	var tmp, _ = returnTwoValue(1,2)
	fmt.Println(tmp)
}
```

여러 변수들을 반환하기로 했던 함수에서 특정 반환값만 이용하려면 ```_```로 해당 변수를 사용하지 않겠다고 해줍니다.