---
layout: post
title: Golang 테스트 프레임워크 GoConvey 라이브러리 알아보기
---

오늘은 Golang으로 BDD 스타일 테스트 프레임워크 GoConvey 라이브러리를 알아보려 합니다.

## GoConvey 설치

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

go get으로 GoConvey 패키지를 설치합니다.

```
go get github.com/smartystreets/goconvey
```

홈의 go 폴더에 GoConvey 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package test
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언하지 않고 테스트를 위해 다른 이름을 적습니다.

```go
import (
	"testing"

	"github.com/smartystreets/goconvey/convey"
)
```

testing과 convey를 가져옵니다.

```go
func TestFunc(t *testing.T) {

	convey.Convey("starting value", t, func() {
		x := 0

		convey.Convey("incremented", func() {
			x++

			convey.Convey("ShouldEqual", func() {
				convey.So(x, convey.ShouldEqual, 1)
			})
		})
	})
```

테스트 함수를 만들고 값이 증가되었을 때 지정한 값과 같은지 검사할 수 있습니다.

```go
	convey.Convey("Numeric comparison", t, func() {
		x := 1
		convey.So(x, convey.ShouldBeGreaterThan, 0)
		convey.So(x, convey.ShouldBeLessThan, 2)
```

지정한 값보다 크거나 작은지 검사할 수 있습니다.

```go
		convey.So(x, convey.ShouldBeBetween, 0, 2)
		convey.So(x, convey.ShouldNotBeBetween, 5, 10)
	})
```

두 수 사이에 존재하는지 검사할 수 있습니다.

```go
	convey.Convey("Container", t, func() {
		intArr := []int{1, 2, 3}
		intMap := map[int]int{1: 1, 2: 2, 3: 3}

		convey.So(intArr, convey.ShouldContain, 2)
		convey.So(intArr, convey.ShouldNotContain, 4)
		convey.So(intMap, convey.ShouldContainKey, 2)
		convey.So(intMap, convey.ShouldNotContainKey, 4)
```

슬라이스와 맵으로 수가 포함되는지 검사할 수 있습니다.

```go
		convey.So(1, convey.ShouldBeIn, []int{1, 2, 3})
		convey.So(0, convey.ShouldNotBeIn, []int{1, 2, 3})
```

슬라이스에 포함된 수에서 존재하는지 검사할 수 있습니다.

```go
		convey.So([]int{}, convey.ShouldBeEmpty)
		convey.So([]int{1}, convey.ShouldNotBeEmpty)
	})
```

비어있는지에 대한 여부를 검사할 수 있습니다.

```go
	convey.Convey("String", t, func() {
		convey.So("helloworld", convey.ShouldStartWith, "h")
		convey.So("helloworld", convey.ShouldNotStartWith, "e")
		convey.So("helloworld", convey.ShouldEndWith, "world")
		convey.So("helloworld", convey.ShouldNotEndWith, "hello")
```

문자열의 시작과 끝에 어떤 문자가 있는지에 대하여 검사할 수 있습니다.

```go
		convey.So("helloworld", convey.ShouldContainSubstring, "ow")
		convey.So("helloworld", convey.ShouldNotContainSubstring, "worldhello")
	})
```

단순하게 어떤 문자열이 포함되었는지 검사할 수 있습니다.

```go
	convey.Convey("Typechecking", t, func() {
		convey.So(10, convey.ShouldHaveSameTypeAs, 0)
		convey.So(10, convey.ShouldNotHaveSameTypeAs, "10")
	})
}
```

검사하려는 값의 타입을 비교할 수 있습니다.