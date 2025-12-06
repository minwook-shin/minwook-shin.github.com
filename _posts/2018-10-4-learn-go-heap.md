---
layout: post
title: Go 언어 heap 컨테이너 대하여 배워보기 
---

오늘은 Go 언어 컨테이너에 포함되있는 heap을 작성하면서 배워보려 합니다.


```go
package main

import (
	"container/heap"
	"fmt"
)
```

container/heap 패키지를 가져와서 인터페이스 타입에서 미구현된 함수들을 작성해주어야 합니다.

```go
type intHeap []int

func (h intHeap) Len() int { 
	return len(h) 
}
func (h intHeap) Less(i, j int) bool { 
	return h[i] < h[j] 
}
func (h intHeap) Swap(i, j int) {
	h[i], h[j] = h[j], h[i] 
}
func (h *intHeap) Push(x interface{}) {
	*h = append(*h, x.(int))
}
func (h *intHeap) Pop() interface{} {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[0 : n-1]
	return x
}
```

미구현된 Len, Less, Swap, Push, Pop 함수를 구현해줍니다.

위 코드는 [golang.org](https://golang.org/pkg/container/heap/)의 패키지 설명의 예제코드를 가져왔습니다.

Len 함수는 heap의 크기를 반환해주고, Less 함수는 원소들의 크기를 비교합니다.

Swap 함수는 각 원소들을 바꿔주며, Push와 Pop함수로 값을 넣고 뺄 수 있습니다.

```go
func main() {
	h := &intHeap{10, 1, 5}
	heap.Init(h)
	heap.Push(h, 20)
	heap.Push(h, 15)
```

메인함수에서는 정수형 heap을 만들어서 할당해줍니다.

그리고 push로 원하는 heap에 값을 넣어줍니다.

```go
	for h.Len() > 0 {
		fmt.Printf("%d ", heap.Pop(h))
	}
}
```

for문으로 heap의 크기가 0이 될 때까지 반복하면서 pop해줍니다.

작은 값부터 정렬되어 출력됩니다.