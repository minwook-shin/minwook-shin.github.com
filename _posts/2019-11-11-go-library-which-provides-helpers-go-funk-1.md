---
layout: post
title: Golang 다양한 함수 유틸리티 go-funk 라이브러리 알아보기 1
---

오늘은 Golang으로 map, find, contains, filter, chunk, reverse와 같은 여러 함수들을 지원하는 go-funk 라이브러리를 알아보려 합니다.

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
	contains := funk.Contains([]string{"hello", "world"}, "hello")
	fmt.Println(contains)
```

Contains 함수로 특정 문자열이 포함되었는지 확인할 수 있습니다.

```go
	testStruct := &test{
		ID:       1,
		Password: "0000",
	}
	contains = funk.Contains([]*test{testStruct}, testStruct)
	fmt.Println(contains)
	contains = funk.Contains([]*test{testStruct}, nil)
	fmt.Println(contains)
```

Contains 함수로 구조체가 포함되었는지 확인할 수 있습니다.

```go
	testStruct2 := &test{
		ID:       2,
		Password: "1111",
	}

	get := funk.Get(testStruct2, "ID")
	fmt.Println(get)
```

구조체의 경로에 있는 값을 가져올 수 있습니다.

```go
	contains = funk.Contains([]*test{testStruct}, testStruct2)
	fmt.Println(contains)
```

새로 만든 객체로 포함되었는 지 확인할 수 있습니다.

```go
	contains = funk.Contains("hello", "lo")
	fmt.Println(contains)
	contains = funk.Contains("hello", "world")
	fmt.Println(contains)
```

문자열에서도 일부 문자가 포함되었는 지 확인할 수 있습니다.

```go
	contains = funk.Contains(map[int]string{1: "test"}, 1)
	fmt.Println(contains)
```

맵으로도 포함되었는 지 확인할 수 있습니다.

```go
	indexOf := funk.IndexOf([]string{"hello", "world"}, "hello")
	fmt.Println(indexOf)
	indexOf = funk.IndexOf([]string{"hello", "world"}, "test")
	fmt.Println(indexOf)
```

IndexOf로 어느 인덱스에 존재하는 지 확인할 수 있습니다.

만약 존재하지 않는다면 -1을 출력합니다.

```go
	indexOf = funk.LastIndexOf([]string{"hello", "hello", "world"}, "hello")
	fmt.Println(indexOf)
	indexOf = funk.LastIndexOf([]string{"hello", "world"}, "test")
	fmt.Println(indexOf)
```

LastIndexOf로 일치하는 마지막 인덱스를 확인할 수 있습니다.

```go
	results := []*test{testStruct, testStruct2}
	mapping := funk.ToMap(results, "ID")
	fmt.Println(mapping)
```

ToMap으로 특정 키를 기반으로 객체를 map 변환할 수 있습니다.

```go
	filter := funk.Filter([]int{1, 2, 3, 4}, func(x int) bool {
		return x%2 == 0
	})
	fmt.Println(filter)
```

Filter로 원하는 조건에 일치하는 값들을 반환할 수 있습니다.

```go
	find := funk.Find([]int{1, 2, 3, 4}, func(x int) bool {
		return x%2 == 0
	})
	fmt.Println(find)
```

Find로 왼쪽부터 순서대로 조건에 맞는 값을 찾아서 반환할 수 있습니다.

```go
	intMap := funk.Map([]int{1, 2, 3, 4}, func(x int) int {
		return x * 2
	})
	fmt.Println(intMap)

	stringMap := funk.Map([]int{1, 2, 3, 4}, func(x int) string {
		return "Hello"
	})
	fmt.Println(stringMap)

	intMap = funk.Map([]int{1, 2, 3, 4}, func(x int) (int, int) {
		return x, x
	})
	fmt.Println(intMap)
```

Map으로 반복 가능한 값에서 모든 요소를 원하는 값들로 변환할 수 있습니다.

```go
	mapping = map[int]string{
		1: "hello",
		2: "world",
	}

	intMap = funk.Map(mapping, func(k int, v string) int {
		return k
	})
	fmt.Println(intMap)

	stringMap = funk.Map(mapping, func(k int, v string) (string, string) {
		return fmt.Sprintf("%d", k), v
	})
	fmt.Println(stringMap)
}
```

Map으로 map을 변형시킬 수 있습니다.

> 내일 다음 포스트가 업로드됩니다.