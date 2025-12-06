---
layout: post
title: Golang 값 비교하는 go-cmp 라이브러리 알아보기
---

오늘은 Golang으로 테스트할 때에 값을 비교하는 go-cmp 라이브러리를 알아보려 합니다.

## go-cmp 설치

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

go get으로 go-cmp 패키지를 설치합니다.

```
go get -u github.com/google/go-cmp/cmp
```

홈의 go 폴더에 go-cmp 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"math"
	"sort"

	"github.com/google/go-cmp/cmp"
)
```

fmt, math, sort와 go-cmp를 가져옵니다.

```go
type wifi struct {
	SSID string
	IP   string
}
```

wifi라는 구조체를 만듭니다.

```go
func wifiList() (x, y wifi) {
	x = wifi{
		SSID: "Free_WIFI",
		IP:   "192, 168, 0, 1",
	}
	y = wifi{
		SSID: "Free_WIFI",
		IP:   "192, 168, 0, 2",
	}
	return x, y
}
```

wifi 객체를 만들어서 서로 비교할 수 있게 함수의 반환 값을 구성합니다.

```go
func main() {
	original, get := wifiList()
	if diff := cmp.Diff(original, get); diff != "" {
		fmt.Println(diff)
	}
```

wifiList 함수로 인하여 서로 비교할 수 있는 객체를 가져와 Diff 함수로 다른 점을 비교합니다.

```go
	option := cmp.Comparer(func(x, y float64) bool {
		return (math.IsNaN(x) && math.IsNaN(y)) || x == y
	})
```

비교하는 방식을 정의하는 함수로 옵션을 만듭니다.

```go
	x := []float64{1, math.NaN(), math.E, 0.0}
	y := []float64{1, math.NaN(), math.E, 0.0}

	fmt.Println(cmp.Equal(x, y, option))
```

미리 만들어둔 옵션을 가지고 두 float64 슬라이스를 같은지 비교합니다.

```go
	option = cmp.Transformer("Sort", func(i []int) []int {
		arr := append([]int(nil), i...)
		sort.Ints(arr)
		return arr
	})
```

변수를 변환하는 Transformer 함수로 정렬하는 옵션을 만듭니다.

```go
	originArr := struct{ Ints []int }{[]int{0, 1, 2, 3, 4, 5}}
	copyArr := struct{ Ints []int }{[]int{2, 0, 1, 4, 3, 5}}

	fmt.Println(cmp.Equal(originArr, copyArr, option))
}
```

미리 만들어둔 옵션으로 정렬하고, 두 float64 슬라이스를 같은지 비교합니다.