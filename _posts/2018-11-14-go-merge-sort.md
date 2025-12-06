---
layout: post
title: Go 언어 병합 정렬 구현하기
---

오늘은 Go 언어로 병합 정렬을 구현해보려 합니다.

```go
package main

import (
	"fmt"
)
```

병합 정렬을 하면서 정렬된 배열을 출력하기 위해 fmt 패키지를 가져옵니다.

```go
func sort(arr []int) []int {
	return divide(arr)
}
```

정렬할 배열을 분할 함수에 넘겨주며 반환합니다.

```go
func divide(arr []int) []int {
	if len(arr) < 2 {
		return arr
	}
	middle := len(arr) / 2
	return merge(divide(arr[:middle]), divide(arr[middle:]))
}
```

일단 배열을 절반으로 나누어 병합해주는 함수를 재귀적으로 작성합니다.

그러면 각 배열이 2보다 작을 때까지 계속 재귀적으로 나누어지고, if문에 의해 재귀 함수를 탈출하게 되면 바로 정렬을 위한 병합 작업에 들어가게 되면서 반환합니다.

```go
func merge(l []int, r []int) []int {
	arr := make([]int, len(l)+len(r))
```

병합하는 함수에서는 왼쪽과 오른쪽 정수형 배열을 받아서 한 배열로 합쳐지게 됩니다.

우선 왼쪽과 오른쪽 배열을 병합해줄 전체 크기의 배열을 만들어줍니다.

```go
	for i, j, all := 0, 0, 0; all < len(l)+len(r); all++ {
		if i < len(l) && j >= len(r) {
			arr[all] = l[i]
			i++
			continue
		}
		if j < len(r) && i >= len(l) {
			arr[all] = r[j]
			j++
			continue
		}
		if l[i] < r[j] {
			arr[all] = l[i]
			i++
			continue
		}
		if l[i] >= r[j] {
			arr[all] = r[j]
			j++
			continue
		}
    }
```

반환할 전체 배열과 왼쪽 배열, 오른쪽 배열의 인덱스를 0으로 초기화한 다음에 왼쪽과 오른쪽을 비교하여 작은 것을 먼저 넣어주고, 오른쪽과 왼쪽 배열의 크기를 비교해가며 남는 숫자를 넣어줍니다.

위 작업을 거치면서 전체 배열의 인덱스는 앞으로 나아가게 됩니다.

```go
	return arr
}
```

for문이 끝나면 왼쪽과 오른쪽 두 배열이 합쳐진 배열이 나오게 되고, 정렬된 배열을 반환하게 됩니다.

```go
func main() {
	arr := []int{1, 4, 6, 7, 8, 6, 4, 3, 2}
	fmt.Println(arr)
	fmt.Println(sort(arr))
}
```

메인 함수에서는 정수형 배열을 준비해주며, 이를 정렬하는 함수에 넣어 분할하고 병합하는 과정을 거쳐 배열 정렬이 됩니다.