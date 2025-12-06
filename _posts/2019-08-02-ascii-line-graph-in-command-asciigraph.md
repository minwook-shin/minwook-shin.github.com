---
layout: post
title: Golang 터미널 그래프 출력하는 asciigraph 라이브러리 알아보기
---

오늘은 Golang으로 터미널에서 ASCII 그래픽을 이용한 그래프 그리는 asciigraph 라이브러리를 알아보려 합니다.

## asciigraph 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 github에서 호스팅되고 있는 asciigraph 패키지를 설치합니다.

```
go get github.com/guptarohit/asciigraph
```

홈의 go 폴더에 asciigraph 소스코드와 패키지 파일이 생성됩니다.

## 증가 그래프 예제

간단하게 증가하는 그래프를 그려보려 합니다.

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/guptarohit/asciigraph"
)
```

fmt와 asciigraph를 가져옵니다.

```go
func main() {
	data := []float64{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
```

float64 슬라이스로 이루어진 데이터를 준비합니다.

```go
	graph := asciigraph.Plot(data, asciigraph.Width(100), asciigraph.Caption("increase"))
```

asciigraph.Plot으로 그래프를 그리고, Width과 Caption도 설정합니다.

height, width, offset, caption에 대해서 설정할 수 있습니다.

```go
	fmt.Println(graph)
}
```

마무리로 그래프를 터미널에 출력합니다.

## sin 그래프 곡선 예제

sin 곡선 그래프를 그려보려 합니다.

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"math"

	"github.com/guptarohit/asciigraph"
)
```

fmt, math 그리고 asciigraph를 가져옵니다.

```go
func main() {
	var data2 []float64
```

float64 슬라이스 변수를 생성합니다.

```go
	for i := 0; i < 100; i++ {
		data2 = append(data2, math.Sin(float64(i)*((math.Pi*1)/10)))
	}
```

생성한 float64 슬라이스에 sin 곡선을 그릴 준비를 합니다.

```go
	graph2 := asciigraph.Plot(data2, asciigraph.Height(10), asciigraph.Caption("sin"))
```

asciigraph.Plot으로 그래프를 그리고, Height과 Caption도 설정합니다.

height, width, offset, caption에 대해서 설정할 수 있습니다.

```go
	fmt.Print(graph2)
}
```

마무리로 그래프를 터미널에 출력합니다.
