---
layout: post
title: Go 언어 기수 정렬 구현하기
---

오늘은 Go 언어로 기수 정렬을 구현해보려 합니다.

```go
package main

import (
	"fmt"
)
```

기수 정렬을 하면서 정렬된 배열을 출력하기 위해 fmt 패키지를 가져옵니다.

```go
func findLNum(arr []int) int {
	n := 0
	for _, j := range arr {
		if j > n {
			n = j
		}
	}
	return n
}
```

배열에서 가장 큰 수를 반환하는 함수를 작성해주어 기수 정렬을 할 때에 가장 큰 수의 자리까지 비교하도록 합니다.

```go
func radix(arr []int) []int {
	largestNum := findLNum(arr)
	digit := 1
	temp := make([]int, len(arr))
```

인자로 받고, 반환하는 기수 정렬을 수행하는 함수를 작성합니다.

가장 큰 수를 찾고, 자리수를 1로 초기화합니다.

```go
	for largestNum > digit {
		bucket := []int{0, 0, 0, 0, 0, 0, 0, 0, 0, 0}
		for _, j := range arr {
			bucket[(j/digit)%10]++
		}
		for i := 1; i < 10; i++ {
			bucket[i] += bucket[i-1]
        }
```

최대의 자리수만큼 반복하면서, 버킷을 만들어줍니다.

각 버킷에 들어갈 숫자의 수를 세고, 이전 버킷의 개수를 추가합니다.

```go
		for i := len(arr) - 1; i >= 0; i-- {
			bucket[(arr[i]/digit)%10]--
			temp[bucket[(arr[i]/digit)%10]] = arr[i]
        }
```

버킷을 사용하여 임시 배열을 채워줍니다.

```go
        copy(arr, temp)
```

반환하기로 한 배열에 임시 배열을 복사합니다.

```go
        digit *= 10
    }
```

자리수를 다음 자리로 옮깁니다.

```go
	return arr
}
```

for문이 끝나면 정렬도 마무리되어 반환하게 됩니다.

```go
func main() {
	arr := []int{9999, 5045, 5043, 145, 424, 613, 785, 835, 642, 413, 336, 253, 3000}
	fmt.Println(arr)
	fmt.Println(radix(arr))
}
```

메인 함수에서 정렬하고 출력합니다.