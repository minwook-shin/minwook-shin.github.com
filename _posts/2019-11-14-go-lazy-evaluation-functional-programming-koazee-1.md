---
layout: post
title: Golang 슬라이스 처리 Koazee 라이브러리 알아보기 1
---

오늘은 Golang으로 따로 함수를 만들지 않고 Stream 처럼 작업하는 Koazee 라이브러리를 알아보려 합니다.

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
	stream := koazee.StreamOf(intArray)
	fmt.Println(stream.Out().Val())
```

정수 슬라이스를 스트림에 불러옵니다.

스트임의 슬라이스를 출력합니다.

```go
	stream = koazee.StreamOf(intArray)
	fmt.Println(stream.At(1).Int())
```

선택한 인덱스의 값을 정수로 출력합니다.

```go
	fmt.Println(stream.First().Int())

	fmt.Println(stream.Last().Int())
```

처음이나 마지막 수를 정수로 출력합니다.

```go
	stream = koazee.StreamOf(intArray)
	fmt.Println(stream.Add(10).Do().Out().Val())
```

스트림에 새로운 수를 추가합니다.

```go
	fmt.Println(stream.Drop(1).Do().Out().Val())
```

입력된 수를 스트림에서 제거합니다.

```go
	fmt.Println(stream.DropWhile(
		func(element int) bool {
			return element <= 5
		}).Do().Out().Val())
```

해당 조건과 일치하지 않는 수들을 제거합니다.

```go
	fmt.Println(stream.DeleteAt(0).Do().Out().Val())
```

선택한 인덱스의 값을 제거합니다.

```go
	fmt.Println(stream.Set(0, 10).Do().Out().Val())
```

스트림에서 특정 위치에 특정 수를 정합니다.

```go
	output, returnStream := stream.Pop()
	fmt.Println(output.Int())
	fmt.Println(returnStream.Out().Val())
```

꺼낸 수와 남은 스트림을 동시에 가져와서 출력합니다.

```go
	stream = koazee.StreamOf(intArray)
	count, _ := stream.Count()
	fmt.Println(count)
```

스트림에 존재하는 수의 갯수를 셉니다.

```go
	indexOf, _ := stream.IndexOf(1)
	fmt.Println(indexOf)

	indexesOf, _ := stream.IndexesOf(1)
	fmt.Println(indexesOf)

	lastIndexOf, _ := stream.LastIndexOf(1)
	fmt.Println(lastIndexOf)
}
```

특정 수가 스트림에 있으면 인덱스를 가져와 출력합니다.

> 내일 다음 포스트가 업로드됩니다.