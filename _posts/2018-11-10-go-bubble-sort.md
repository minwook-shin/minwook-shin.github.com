---
layout: post
title: Go 언어 버블 정렬 구현하기
---

오늘은 Go 언어로 버블 정렬을 구현해보려 합니다.

```go
package main

import (
	"fmt"
)
```

정렬을 한 배열을 출력하기 위해 fmt 패키지를 가져옵니다.

```go
func bubble(arr []int) []int {
```

int 배열로 받고 int 배열로 반환하는 bubble 이라는 함수를 만들어줍니다.

```go
	for i := 0; i < len(arr); i++ {
		for j := 0; j < len(arr)-1; j++ {
			if arr[j] > arr[j+1] {
				arr[j], arr[j+1] = arr[j+1], arr[j]
			}
		}
	}
	return arr
}
```

이중 for문으로 반복하면서 j번째와 j+1번째를 계속 비교하면서 앞의 j번째가 더 크면 서로 교체해주면서 최종적으로는 가장 큰 값을 맨 뒤로가게 합니다.

그리고 이와 같은 방법을 배열의 크기만큼 i번 반복하면 배열이 정렬되게 됩니다.

```go
func main() {
    arr := []int{3, 7, 4, 1, 7, 5, 4, 8}
    fmt.Println(arr)
	fmt.Println(bubble(arr))
}
```

메임 함수에서 []int 타입을 함수의 인자로 넣어주면 정렬된 배열로 반환됩니다.