---
layout: post
title: Go 언어 쉘 정렬 구현하기
---

오늘은 Go 언어로 쉘 정렬을 구현해보려 합니다.

```go
package main

import (
	"fmt"
)
```

정렬된 배열을 출력하기 위해 fmt 패키지를 가져옵니다.

```go
func insertion(arr []int, h int) {
	for i := h; i < len(arr); i++ {
		for j := i; j > h-1 && arr[j-h] > arr[j]; j = j - h {
			arr[j], arr[j-h] = arr[j-h], arr[j]
		}
	}
}
```

삽입 정렬을 개선하기 위해 h 간격만큼 벌어져있는 인덱스끼리 삽입정렬을 수행합니다.

```go
func shell(arr []int) []int {
	h := 1
	for h < len(arr)/3 {
		h = 3*h + 1
    }
```

쉘 정렬을 할 때에 간격을 증가하는 식은 ```3 * h + 1```이 효율이 좋다고 전해집니다.

for문으로 자신의 배열 길이에 맞는 간격을 찾아줍니다.

```go
	for h > 0 {
		insertion(arr, h)
		h = (h - 1) / 3
    }
```

간격을 찾으면 해당 간격만큼 벌어져 있는 인덱스끼리 삽입 정렬을 수행합니다.

간격이 0이 되기 전까지 간격을 감소시키면서 for문으로 반복합니다.

```go
	return arr
}
```

배열이 정렬되면 함수가 반환되기 전에 배열을 반환합니다.

```go
func main() {
	arr := []int{3, 7, 4, 1, 7, 5, 4, 8}
	fmt.Println(arr)
	fmt.Println(shell(arr))
}
```

메인 함수에서 해당 쉘 정렬 함수로 정수형 인자를 넘겨주고, 정렬된 배열을 반환받습니다.

정렬된 배열을 출력해봅니다.