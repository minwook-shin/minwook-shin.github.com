---
layout: post
title: Go 언어 힙 정렬 구현하기
---

오늘은 Go 언어로 힙 정렬을 구현해보려 합니다.

```go
package main

import "fmt"
```

힙 정렬을 하면서 정렬된 배열을 출력하기 위해 fmt 패키지를 가져옵니다.


```go
func sort(arr []int) []int {
	for i := len(arr) / 2; i >= 0; i-- {
		heapify(arr, i, len(arr))
	}
```

배열 마지막 인덱스에 해당되는 부모 노드부터 역순으로 정렬을 위한 heapify 연산을 시작합니다.

heapify 함수로 힙 성질에 맞도록 배열을 재배열해줍니다.

```go
	for i := len(arr); i > 1; i-- {
		arr[0], arr[i-1] = arr[i-1], arr[0]
		heapify(arr, 0, i-1)
	}
	return arr
}
```

배열의 루트에 있는 인덱스랑 마지막 인덱스를 바꾸어주며 배열의 크기를 1 줄인 상태로 힙을 재배열합니다.

그러면 오름차순으로 정렬됩니다.

정렬된 배열을 반환합니다.

```go
func heapify(arr []int, root, length int) {
	var max = root
	var l, r = (root * 2) + 1, (root * 2) + 2
```

이제 heapify 함수를 구현하려 합니다. 

우선 루트를 최대 값이라고 두고, (root * 2) + 1는 왼쪽 자식 노드, (root * 2) + 2는 오른쪽 자식 노드라고 정의합니다.

```go
	if l < length && arr[l] > arr[max] {
		max = l
	}

	if r < length && arr[r] > arr[max] {
		max = r
	}

	if max != root {
		arr[root], arr[max] = arr[max], arr[root]
		heapify(arr, max, length)
	}
}
```

루트의 왼쪽 자식 노드 l번째에 있는 것이 가장 크다면, l을 크다고 지정하고 반대로 루트의 오른쪽 자식 노드 r번째에 있는 것이 가장 크다면, r을 크다고 지정합니다.

만약 큰 게 루트가 아니라면 큰 값이랑 루트랑 바꿔줍니다.

```go
func main() {
	arr := []int{9999, 5045, 5043, 145, 424, 613, 785, 835, 642, 413, 336, 253, 3000}

	fmt.Println(arr)
	fmt.Println(sort(arr))
}
```

이전 배열 포스팅처럼 메인 함수를 작성해줍니다.

배열을 정렬하고 출력합니다.