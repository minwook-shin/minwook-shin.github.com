---
layout: post
title: Go 언어 타입에 대하여 배워보기
---

오늘은 Go 언어의 타입에 대하여 작성하면서 배워보려 합니다.

go 언어에는 타입이 크게 bool, numeric, string으로 구별되어 있습니다.

## bool

```go
package main

import (
	"fmt"
)

func main() {
	a := true
	b := false
	fmt.Println(a,b)
	fmt.Println(a&&b)
	fmt.Println(a||b)
}
```

변수에 true, false로 부여할 수 있습니다.

## numeric

* Signed integers
    * int8: 8 비트 / -128 ~ 127
    * int16: 16 비트 / -32768 ~ 32767
    * int32: 32 비트 / -2147483648 ~ 2147483647
    * int64: 64 비트 / -9223372036854775808 ~ 9223372036854775807
    * int: 32 비트 or 64 bit / -2147483648 ~ 2147483647 or -9223372036854775808 ~ 9223372036854775807

```go
package main

import (
	"unsafe"
	"reflect"
	"fmt"
)

func main() {
	var c int64 
	c = 9223372036854775807
	d := 9223372036854775807
	fmt.Println(reflect.TypeOf(c))
	fmt.Println(reflect.TypeOf(d))
	fmt.Println(unsafe.Sizeof(c))
	fmt.Println(unsafe.Sizeof(d))
}
```

예제코드를 작성한 컴퓨터가 64비트이므로 int형이 64비트이므로 223372036854775807 까지 지원함을 확인할 수 있습니다.

참고로 reflect의 TypeOf으로 변수의 타입을 확인할 수 있으며, unsafe의 Sizeof로 변수의 크기를 확인할 수 있습니다.

* Unsigned integers
    * uint8: 8 비트 / 0 ~ 255
    * uint16: 16 비트 / 0 ~ 65535
    * uint32: 32 비트 / 0 ~ 4294967295
    * uint64: 64 비트 / 0 ~ 18446744073709551615
    * uint : 32 비트 or 64 비트 / 0 ~ 4294967295 or 0 ~ 18446744073709551615

```go
package main

import (
	"unsafe"
	"fmt"
)

func main() {
	var e uint
	e = 18446744073709551615
	fmt.Println(unsafe.Sizeof(e))
}
```

타 언어에서 보통 unsigned int라고 불리는 것을 go에서는 uint라고 적습니다.

이 예제코드 역시 64비트 컴퓨터에서 작성했으므로 64비트인 18446744073709551615까지 출력됨을 확인할 수 있습니다.

* Floating point types
    * float32: 32 비트
    * float64: 64 비트

```go
package main

import (
	"fmt"
)

func main() {
	f,g := 3.14, 3.15
	fmt.Println(f,g)
	tmp1 := f+g
	tmp2 := f-g
	fmt.Println(tmp1,tmp2)
}
```

실수는 부동 소수점으로 지원합니다.

* Complex types
    * complex64: complex numbers which have float32 real and imaginary parts
    * complex128: complex numbers with float64 real and imaginary parts

```go
package main

import (
	"fmt"
)

func main() {
	tmp3 := complex(1,5)
	tmp4 := 1+5i
	fmt.Println(tmp3,tmp4)
}
```

복소수 또한 지원합니다.

complex 함수를 사용하여 Complex 타입을 사용할 수 있습니다.


* rune

```go
package main

import (
	"fmt"
)

func main() {
	var z = 'A'
	fmt.Println(z)
}
```

UTF-8 character를 int형으로 저장할 수 있는 rune 타입입니다.

## string

```go
package main

import (
	"fmt"
)

func main() {
    var name string 
	name = "string"
	fmt.Println(name)
}
```

문자열을 저장할 수 있는 string 타입입니다.

## 타입 변환 

```go
package main

import (
	"fmt"
)

func main() {
	var(i ,j =1, 3.14)
	fmt.Println(i + int(j))
}
```

go 언어는 명시적 형변환을 지원하므로 위와 같이 변수 타입을 변환할 수 있습니다.