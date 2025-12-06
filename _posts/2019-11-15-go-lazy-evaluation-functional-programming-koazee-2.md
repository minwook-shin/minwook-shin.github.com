---
layout: post
title: Golang 슬라이스 처리 Koazee 라이브러리 알아보기 2
---

오늘도 Golang으로 따로 함수를 만들지 않고 Stream 처럼 작업하는 Koazee 라이브러리를 알아보려 합니다.

## Koazee 설치

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

go get으로 Koazee 패키지를 설치합니다.

```
go get github.com/wesovilabs/koazee
```

홈의 go 폴더에 Koazee 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/wesovilabs/koazee"
)
```

fmt 그리고 koazee를 가져옵니다.

```go
func main() {
	intArray := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
	fmt.Println(intArray)
```

스트림처럼 만들어서 조작할 정수 슬라이스를 준비합니다.

```go
	contains, _ := stream.Contains(1)
	fmt.Println(contains)
```

특정 수가 스트림에 있는 지 확인합니다.

```go
	stream = koazee.StreamOf(intArray)
	fmt.Println(stream.Reverse().Out().Val())
```

스트림을 역순으로 다시 정렬합니다.

```go
	stream = koazee.StreamOf(intArray)
	fmt.Println(stream.Take(1, 3).Out().Val())
```

두 인덱스의 사이에 있는 값들을 슬라이스로 출력합니다.

```go
	fmt.Println(stream.
		Filter(
			func(val int) bool {
				return val > 3
			}).
		Out().Val(),
	)
```

특정 조건에 맞지 않는 수들을 스트림에서 삭제하고 출력합니다.

```go
	fmt.Println(stream.RemoveDuplicates().Out().Val())
```

스트림에서 중복되는 수들을 삭제하고 출력합니다.

```go
	stream = koazee.StreamOf(intArray)
	value, _ := stream.GroupBy(func(val int) int { return val * 10 })
	fmt.Println(value)
```

반환 조건대로 그룹을 묶어 map으로 변환하고 출력합니다.

```go
	stream = koazee.StreamOf(intArray)
	fmt.Println(stream.Reduce(func(acc, val int) int {
		return acc + val
	}).Int())
```

값을 계산할 수 있습니다.

```go
	stream = koazee.StreamOf(intArray)
	stream.ForEach(func(i int) {
		fmt.Println(i)
	}).Do()

	stream = koazee.
		StreamOf(intArray).
		Filter(func(i int) bool { return i > 3 }).
		ForEach(func(i int) {
			fmt.Println(i)
		})
	stream.Do()
}
```

Filter로 걸러진 정수 슬라이스의 스트림을 가지고 ForEach로 반복하여 이어 출력합니다.