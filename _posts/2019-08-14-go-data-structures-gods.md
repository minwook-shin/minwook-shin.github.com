---
layout: post
title: Golang 자료구조와 알고리즘 구현하는 GoDS 라이브러리 알아보기
---

오늘은 Golang으로 자료구조와 알고리즘이 구현되어 있어서 쉽게 사용할 수 있는 GoDS((Go Data Structures) 라이브러리를 알아보려 합니다.

## GoDS 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 GoDS 패키지를 설치합니다.

```
go get github.com/emirpasic/gods
```

홈의 go 폴더에 GoDS 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/emirpasic/gods/lists/singlylinkedlist"
	"github.com/emirpasic/gods/maps/hashmap"
	"github.com/emirpasic/gods/sets/hashset"
	"github.com/emirpasic/gods/stacks/linkedliststack"
	"github.com/emirpasic/gods/trees/avltree"
	"github.com/emirpasic/gods/utils"
)
```

linkedlist, hashmap, stack, tree 등을 가져와서 사용할 수 있습니다.

```go
func main() {
	list := singlylinkedlist.New()
	list.Add("b", "a")
	list.Sort(utils.StringComparator)
	fmt.Println(list.Get(0))
	result := list.Contains("a", "b")
	fmt.Println(result)
	list.Swap(0, 1)
	list.Clear()
	fmt.Println(list.Empty(), list.Size())
	list.Insert(0, "b")
	list.Insert(0, "a")
	it := list.Iterator()
	for it.Next() {
		index, value := it.Index(), it.Value()
		fmt.Println(index, value)
	}
```

singlylinkedlist로 Add, Remove, Insert, Sort, Contains, Swap, Clear 그리고 Iterator 메소드를 사용할 수 있습니다.

Iterator로 인덱스와 값을 알 수 있습니다.

```go
	set := hashset.New()
	set.Add(1, 2, 2, 3, 4, 5)
	set.Remove(4)
	fmt.Println(set.Values())
	set.Contains(1, 5)
	set.Clear()
	fmt.Println(set.Empty(), set.Size())
```

hashset으로 Add, Remove,Remove, Contains, Clear 메소드들을 사용할 수 있습니다.

```go
	stack := linkedliststack.New()
	stack.Push(1)
	stack.Push(2)
	fmt.Println(stack.Values())
	_, _ = stack.Peek()
	_, _ = stack.Pop()
	fmt.Println(stack.Values())
	stack.Clear()
	fmt.Println(stack.Empty(), stack.Size())
```

linkedliststack으로 Push, Peek, Pop 메소드들을 사용할 수 있습니다.

```go
	hashMap := hashmap.New()
	hashMap.Put(1, "x")
	hashMap.Put(2, "b")
	hashMap.Put(1, "a")
	fmt.Println(hashMap.Values(), hashMap.Keys())
	hashMap.Remove(1)
	hashMap.Clear()
	fmt.Println(hashMap.Empty(), hashMap.Size())
```

hashmap으로 Put, Remove 등의 메소드들을 사용할 수 있습니다.

```go
	tree := avltree.NewWithIntComparator()
	tree.Put(1, "a")
	tree.Put(2, "b")
	tree.Put(3, "c")
	tree.Put(4, "d")
	tree.Put(5, "e")
	fmt.Println(tree)
	fmt.Println(tree.Values())
	it2 := tree.Iterator()
	for it2.Next() {
		key, value := it2.Key(), it2.Value()
		fmt.Println(key, value)
	}
	tree.Clear()
	fmt.Println(tree.Empty(), tree.Size())
}
```

avltree로 Put, Remove 그리고 Iterator 메소드를 사용할 수 있으며,

Iterator로 키와 값을 알 수 있습니다.