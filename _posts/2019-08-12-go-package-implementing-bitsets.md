---
layout: post
title: Golang 비트셋 구현하는 bitset 라이브러리 알아보기
---

오늘은 Golang으로 비트셋을 구현한 bitset 라이브러리를 알아보려 합니다.

## bitset 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 bitset 패키지를 설치합니다.

```
go get github.com/willf/bitset
```

홈의 go 폴더에 bitset 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"math/rand"

	"github.com/willf/bitset"
)
```

fmt, rand 그리고 bitset을 가져옵니다.

```go
func main() {
	var bs bitset.BitSet
```

BitSet 객체를 생성합니다.

```go
	num := uint(rand.Intn(50))
	bs.Set(num)
	fmt.Println(bs.String())
```

num번째 비트를 1로 설정하고 문자열로 출력합니다.

```go
	bs.Flip(num)
```

num번째 비트를 반전할 수 있습니다.

```go
	fmt.Print(bs.All())
	fmt.Print("\n")
```

모두 비트가 설정되었거나 비어있으면 참으로 반환할 수 있습니다

```go
	fmt.Print(bs.Any())
	fmt.Print("\n")
```

모든 비트 중에 하나라도 설정되어 있지 않으면 거짓이 반환됩니다.

```go
	fmt.Print(bs.None())
	fmt.Print("\n")
```

모두 비트가 설정되어있지 않거나 비어있으면 참으로 반환할 수 있습니다

```go
	fmt.Print(bs.Len())
```

비트셋의 길이를 반환받을 수 있습니다.

```go
	bs.ClearAll()
	fmt.Println(bs.String())
```

비트셋을 초기화하고 문자열로 출력해봅니다.

```go
	bs.Set(1).Set(2)
	fmt.Println(bs.String())
```

체인으로 엮어서 특정 비트룰 1로 설정할 수도 있습니다.

```go
	for index, error := bs.NextSet(0); error; index, error = bs.NextSet(index + 1) {
		fmt.Println(index)
	}
```

다음 비트셋을 반환하는 iterator로 반복할 수 있습니다.

```go
	if bs.Intersection(bitset.New(100).Set(2)).Count() == 1 {
		fmt.Println("BitSet equivalent of '&'")
	}
}
```

and 연산자에 해당하는 집합으로 해당 값이 집합에 포함한다면 1을 반환하여 조건문에 사용할 수 있습니다.