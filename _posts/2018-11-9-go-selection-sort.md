---
layout: post
title: Go 언어 선택 정렬 구현하기
---

오늘은 Go 언어로 선택 정렬을 구현해보려 합니다.

```go
package main

import (
	"fmt"
)
```

정렬된 배열을 출력하기 위해 fmt 패키지를 가져옵니다.

```go
func selection(arr []int) {
	for i := 0; i < len(arr)-1; i++ {
		m := i
		for j := i + 1; j < len(arr); j++ {
			if arr[j] < arr[m] {
				m = j
			}
        }
```

우선 배열의 처음 자리를 선택하고, 다음으로 for문을 한번 더 해서 배열에서 작은 수를 찾습니다.

```go
		arr[m], arr[i] = arr[i], arr[m]
	}
}
```

첫번째로 선택한 i번째와 다시 for문으로 반복해서 찾은 가장 작은 수 m번째를 바꿔줍니다.

이를 첫 for문으로 인해 반복하다가 i번째가 마지막에 다다르면 정렬은 종료됩니다.

```go
func main() {
	arr := []int{7,5,3,9,6,4}
	fmt.Println(arr)
	selection(arr)
	fmt.Println(arr)
}
```

main 함수에서는 위와 같이 작성해주면 됩니다.