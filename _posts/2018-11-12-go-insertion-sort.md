---
layout: post
title: Go 언어 삽입 정렬 구현하기
---

오늘은 Go 언어로 삽입 정렬을 구현해보려 합니다.

```go
package main

import (
	"fmt"
)
```

정렬을 한 배열을 출력하기 위해 fmt 패키지를 가져옵니다.

```go
func insertion(arr []int) []int {
	for i := 1; i < len(arr); i++ {
```

배열의 길이정도 반복하면서 i를 1로 주어 첫번째는 이미 정렬됬음을 가정하고 시작합니다.

```go
		for j := i; j > 0 && arr[j-1] > arr[j]; j-- {
			arr[j-1], arr[j] = arr[j], arr[j-1]
		}
    }
```

j를 i에서 가져와서 j가 0보다 크면서 j번째가 j-1번째보다 작을 때만 j를 감소시키면서 서로의 자리를 바꾸어줍니다.

j가 가장 작은 수인 경우에는 for문이 계속 반복되어 첫번째 인덱스까지 도달하게 됩니다.

이를 이중 for문으로 반복하다보면 정렬되어있는 배열의 왼쪽이 점점 늘어나면서 곧 모든 인덱스가 정렬됩니다.

```go
	return arr
}
```

반복이 끝나면 배열을 반환합니다.

```go
func main() {
	arr := []int{3, 7, 4, 1, 7, 5, 4, 8}
	fmt.Println(arr)
	fmt.Println(insertion(arr))
}
```

메인 함수에서 정수형 배열을 준비하고, insertion 함수에 배열을 넣어주면 정렬된 배열이 반환되면서 출력됩니다.