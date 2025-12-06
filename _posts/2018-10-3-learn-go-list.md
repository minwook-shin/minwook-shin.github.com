---
layout: post
title: Go 언어 list 컨테이너 대하여 배워보기 
---

오늘은 Go 언어 컨테이너에 포함되있는 list(더블 링크드리스트)를 작성하면서 배워보려 합니다.

```go
package main

import (
	"container/list"
	"fmt"
)
```

list를 구현하기 위한 패키지를 import해줍니다.

```go
func main() {
    list := list.New()
```

list를 생성하기 위해 New()로 할당해줍니다.

```go
	list.PushBack(1)
	list.PushBack(2)
	list.PushBack(3)
	for i := list.Front(); i != nil; i = i.Next() {
		fmt.Println(i.Value)
    }
```

pushback으로 새로운 내용을 리스트의 뒤에 넣을 수 있습니다.

그리고 내용을 확인하기 위해 for문으로 리스트의 처음부터 마지막까지 nil이 나올 때까지 탐색해줍니다.

```go
	fmt.Println("======")

	list.PushFront(1)
	list.PushFront(2)
	list.PushFront(3)
	for i := list.Front(); i != nil; i = i.Next() {
		fmt.Println(i.Value)
	}
```

pushfront로 새로운 내용을 리스트의 앞에 넣을 수 있습니다.

이 역시 내용을 확인하기 위해 for문으로 리스트의 처음부터 마지막까지 nil이 나올 때까지 탐색해줍니다.

```go
	fmt.Println("======")

	tmp1 := list.PushBack(5)
	list.InsertAfter(4,tmp1)
	for i := list.Front(); i != nil; i = i.Next() {
		fmt.Println(i.Value)
	}
```

InsertAfter를 이용하여 리스트에 할당한 값의 뒤에 특정 값을 넣을 수 있습니다.

```go
	fmt.Println("======")

	tmp2 := list.PushBack(5)
	list.InsertBefore(6,tmp2)
	for i := list.Front(); i != nil; i = i.Next() {
		fmt.Println(i.Value)
	}
}
```

InsertBefore를 이용하여 리스트에 할당한 값의 앞에 특정 값을 넣을 수 있습니다.

이번 포스트에서 모든 입력 값\을 정수로 했지만, 리스트의 값을 나타내는 타입이 interface{}로 되어있기 떄문에 모든 타입을 사용할 수 있습니다.