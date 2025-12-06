---
layout: post
title: Go 언어 퀵 정렬 구현하기
---

오늘은 Go 언어로 퀵 정렬을 구현해보려 합니다.

```go
package main

import (
	"fmt"
)
```

정렬을 한 배열을 출력하기 위해 fmt 패키지를 가져옵니다.

```go
func partition(arr []int) int {
```

인자로 준 배열을 두 파티션으로 분리할 연산이 필요하므로 함수를 따로 작성해줍니다.

```go
	pivot := arr[0]
	start, end := 0, len(arr)-1
```

처음을 피봇으로 주고, 시작(왼쪽)과 끝(오른쪽)을 정해줍니다.

```go
	for {
		for start < len(arr) && arr[start] <= pivot {
			start++
		}
		for arr[end] > pivot {
			end--
        }
```

타 언어에서는 while문이겠지만, golang에서는 for 무한 루프를 선택한 대신 if문으로 탈출하게 됩니다.

그리고 for문 뒤에 조건식을 쓰면 해당 조건식이 성립할 때만 반복되므로, 왼쪽은 피봇보다 작거나 같을 때 증가하고 오른쪽은 피봇보다 클 때 감소합니다.

```go
		if start >= end {
			arr[0], arr[end] = arr[end], arr[0]
			return end
        }
```

위 for 무한 루프를 반복하다가 start와 end가 서로 만나면, 첫번째 값과 오른쪽 값을 바꾸고 함수를 마칩니다.

```go
		arr[start], arr[end] = arr[end], arr[start]
	}
}
```

만약 if문에 맞지 않으면 시작 값과 끝 값을 서로 바꾸어주고 처음으로 돌아가 다시 반복합니다.

```go
func quick(arr []int) {
	if len(arr) > 1 {
		p := partition(arr)
		quick(arr[0:p])
		quick(arr[p+1:])
	}
}
```

파티션을 나누어 주는 함수를 이용하여 정렬하는 함수를 만들어야 합니다.

길이가 1 이상이면 파티션을 나누어주는 함수를 계속 재귀적으로 호출하여 배열 분할이 되도록 작성합니다.

```go
func main() {
	arr := []int{3, 7, 4, 1, 7, 5, 4, 8}
	quick(arr)
	fmt.Println(arr)
}
```

메인 함수에서 정수형 배열을 준비하고 퀵 정렬을 수행하면 정상적으로 정렬이 되는 것을 확인 할 수 있습니다.