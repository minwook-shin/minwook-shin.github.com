---
layout: post
title: Go 언어 패키지에 대하여 배워보기
---

오늘은 Go 언어의 패키지에 대하여 작성하면서 배워보려 합니다.

## 사용자 패키지 만들기

```go
package calc

import "fmt"

func Sum(a,b float64) float64 {
	return a + b
}

func Sub(a,b float64) float64 {
	return a - b
}

func Div(a,b float64) float64 {
	return a / b
}

func Mul(a,b float64) float64 {
	return a * b
}
```

그동안 package 키워드에는 main이라는 이름만 썻지만, 사용할 패키지를 만드려면 폴더 이름을 패키지명으로 써주어야 합니다.

또한 파일이 폴더에 한 개만 있다면 폴더이름이랑 같게 해줍시다.

## 함수 사용 범위

함수명 첫 글자가 대문자면일 때만 타 패키지에서 불러와서 사용할 수 있고 함수명 첫 글자가 소문자이면 해당 패키지 안에서만 실행할 수 있습니다.

```go
func Sum(a,b float64) float64 {
	return a + b
}
```

위와 같이 작성하면 타 패키지에서 사용가능하고,

```go
func sum(a,b float64) float64 {
	return a + b
}
```

위와 같이 작성하면 같은 패키지에서만 사용가능합니다.

## 패키지 사용하기

```go
package main

import (
	"fmt"
	"package/calc"
)

func main() {
	fmt.Println("main")
	fmt.Println(calc.Sum(1.0,1.0))
	fmt.Println(calc.Sub(1.0,1.0))
	fmt.Println(calc.Div(1.0,1.0))
	fmt.Println(calc.Mul(1.0,1.0))
}
```

만든 패키지를 import 해줍니다.

그리고 패키지명으로 지정한 이름에 있는 함수를 사용하면 됩니다.

## init

```go
package main

import (
	"fmt"
)

func init(){
	fmt.Println("main init")
}

func main() {
	fmt.Println("main")
}
```

init 키워드로 프로그램을 실행할 때에 변수 초기화된 뒤 호출되는 함수입니다.

직접 호출할 수 없습니다.

## 빈 식별자

```go
package calc

import "fmt"

func init(){
	fmt.Println("lib init")
}
```

이 패키지의 init을 활용하고 싶지만, 함수는 쓰고 싶지 않을 때에는 ```_```를 이용하여 빈 식별자로 지정합니다.

```go
package main

import (
	"fmt"
	_ "package/calc"
)

func init(){
	fmt.Println("main init")
}

func main() {
	fmt.Println("main")
	// fmt.Println(calc.Sum(1.0,1.0))
	// fmt.Println(calc.Sub(1.0,1.0))
	// fmt.Println(calc.Div(1.0,1.0))
	// fmt.Println(calc.Mul(1.0,1.0))
}
```

평소같이 주석으로 처리하면 해당 패키지를 사용하지 못했다고 나오지만, import할 때 ```_```를 사용하여 빈 식별자로 만들면 해당 패키지의 init 함수만 사용할 수 있습니다. 

## 라이브러리 만들기

```bash
go install calc
```

터미널에서 사용자가 만든 패키지를 위와 같이 작성해주면 pkg라는 폴더에 컴파일된 패키지 라이브러리가 나옵니다.