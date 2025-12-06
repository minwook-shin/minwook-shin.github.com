---
layout: post
title: Go 언어 math/rand 알아보기
---

오늘은 Go 언어 기본 패키지에 포함되있는 math/rand로 랜덤을 수행하는 실습을 해보려 합니다.

```go
package main

import (
    "math/rand"
	"time"
	"fmt"
	"strings"
)
```

난수를 만들어내는 math/rand 패키지를 가져오며, 유닉스 시간의 나노초로 시드를 만들기 위한 time 패키지도 가져옵니다.

출력을 위해 fmt 패키지도 가져옵니다.

```go
func main() {
	rand.Seed(time.Now().UnixNano())
```

유닉스 시간을 나노초로 나타내는 수치를 시드로 삼아줍니다.

```go
	fmt.Println(rand.Intn(100))
	fmt.Println(rand.Int())
	fmt.Println(rand.Int31())
	fmt.Println(rand.Int63())
	fmt.Println(rand.Float32())
	fmt.Println(rand.Float64())
	fmt.Println(rand.Uint32())
	fmt.Println(rand.Uint64())
```

Intn으로 수를 지정하여 int 범위안에 0부터 해당 수까지만 나오게 할 수 있습니다.

그 외로 Int31, Int63 처럼 각각 31비트, 63비트 정수를 반환 함수가 있습니다.

Float32, Float64는 각각 0.0부터 1.0 범위의 수들을 Float형으로 반환합니다.

Uint32, Uint64도 마찬가지로 각각 Uint32, Uint64 형식으로 반환합니다.

```go
	randomShuffle := strings.Fields("hello, world! gopher?")
	rand.Shuffle(len(randomShuffle), func(i, j int) {
		randomShuffle[i], randomShuffle[j] = randomShuffle[j], randomShuffle[i]
	})
	fmt.Println(randomShuffle)
}
```

문자열의 각 단어들을 나누어서 Shuffle을 수행했습니다.

Shuffle 함수의 두번째 인자에는 swap 함수가 들어가므로, i와 j를 서로 바꾸어주는 코드를 작성해주면 됩니다.