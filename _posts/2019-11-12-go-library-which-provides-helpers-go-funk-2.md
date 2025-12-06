---
layout: post
title: Golang 다양한 함수 유틸리티 go-funk 라이브러리 알아보기 2
---

오늘도 이어서 Golang으로 map, find, contains, filter, chunk, reverse와 같은 여러 함수들을 지원하는 go-funk 라이브러리를 알아보려 합니다.

## go-funk 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

맥에서 Golang의 버전을 올리려면 brew를 이용합니다.

```
brew upgrade go
```

우분투에서도 Golang의 버전을 올리려면 backports 저장소를 등록하고 apt를 이용합니다.

```
sudo add-apt-repository ppa:longsleep/golang-backports
sudo apt-get update
sudo apt-get install golang-go
```

go get으로 go-funk 패키지를 설치합니다.

```
go get github.com/thoas/go-funk
```

홈의 go 폴더에 go-funk 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/thoas/go-funk"
)
```

fmt와 go-funk를 가져옵니다.

```go
type test struct {
	ID       int
	Password string
}
```

구조체를 만들어둡니다.

```go
func main() {
	testStruct := &test{
		ID:       1,
		Password: "0000",
	}
```

test라는 구조체로 객체를 만듭니다.

```go
	funk.ForEach([]int{1, 2, 3, 4}, func(x int) {
		fmt.Println(x)
	})
```

ForEach로 반복하여 순서대로 반환되는 값들을 출력할 수 있습니다.

```go
	keys := funk.Keys(map[string]int{"hello": 1, "world": 2})
	fmt.Println(keys)
	keys = funk.Keys(testStruct)
	fmt.Println(keys)
```

Keys로 map 또는 객체의 키, 이름을 반환할 수 있습니다.

```go
	values := funk.Values(map[string]int{"hello": 1, "world": 2})
	fmt.Println(values)
	values = funk.Values(testStruct)
	fmt.Println(values)
```

Values로 map 또는 객체의 값들을 반환할 수 있습니다.

```go
	chunk := funk.Chunk([]int{1, 2, 3, 4, 5}, 2)
	fmt.Println(chunk)
```

Chunk로 두번재 인자에 받는 크기만큼 슬라이스를 잘라 반환할 수 있습니다.

```go
	flattenDeep := funk.FlattenDeep([][]int{[]int{1, 2}, []int{3, 4}})
	fmt.Println(flattenDeep)
```

FlattenDeep으로 쪼개져있는 슬라이스를 합칠 수 있습니다.

```go
	uniq := funk.Uniq([]int{0, 1, 1, 2, 2, 3, 3, 10})
	fmt.Println(uniq)
```

Uniq로 중복을 없앨 수 있습니다.

```go
	drop := funk.Drop([]int{0, 0, 0, 0}, 3)
	fmt.Println(drop)
```

Drop으로 인자로 받는 수 만큼 버리고 남길 수 있습니다.

```go
	intial := funk.Initial([]int{0, 1, 2, 3, 4})
	fmt.Println(intial)
```

Initial로 마지막 수를 제외할 수 있습니다.

```go
	tail := funk.Tail([]int{0, 1, 2, 3, 4})
	fmt.Println(tail)
```

Tail로 첫 수를 제외할 수 있습니다.

```go
	shuffle := funk.Shuffle([]int{0, 1, 2, 3, 4})
	fmt.Println(shuffle)
```

Shuffle로 여러 수들을 섞을 수 있습니다.

```go
	sum := funk.Sum([]int{0, 1, 2, 3, 4})
	fmt.Println(sum)
```

Sum으로 모든 수들을 더할 수 있습니다.

```go
	reverse := funk.Reverse([]int{0, 1, 2, 3, 4})
	fmt.Println(reverse)
```

Reverse로 수들을 섞을 수 있습니다.

```go
	slice := funk.SliceOf(testStruct)
	fmt.Println(slice)
```

SliceOf로 원하는 객체를 슬라이스에 포함하여 반환할 수 있습니다.

```go
	random := funk.RandomInt(0, 100)
	fmt.Println(random)
	random2 := funk.RandomString(4)
	fmt.Println(random2)
```

RandomInt로 원하는 범위의 난수를 구하거나, RandomString으로 지정된 길이의 무작위 문자열을 반환할 수 있습니다.

```go
	shard := funk.Shard("e89d66bdfdd4dd26b682cc77e23a86eb", 2, 2, false)
	fmt.Println(shard)
	shard = funk.Shard("e89d66bdfdd4dd26b682cc77e23a86eb", 2, 2, true)
	fmt.Println(shard)
}
```

Shard로 문자열 이름을 Shard합니다