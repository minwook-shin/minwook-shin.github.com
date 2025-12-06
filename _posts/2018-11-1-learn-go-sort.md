---
layout: post
title: Go 언어 sort 알아보기
---

오늘은 Go 언어 기본 패키지에 포함되있는 sort로 정렬 수행하는 실습을 해보려 합니다.

```go
package main

import "fmt"
import "sort"
```

sort 패키지와 fmt 패키지를 가져와서 정렬하고 출력하게 합니다.

```go
type blog struct {
	Author string
	Text   string
	year   int
}
```

구조체를 만들어서 기본형만 정렬되는 것이 아니고 커스텀된 구조도 정렬이 가능하다는 것을 알 수 있습니다.

```go
type customInterface []blog

func (c customInterface) Len() int {
	return len(c)
}
func (c customInterface) Swap(i, j int) {
	c[i], c[j] = c[j], c[i]
}
func (c customInterface) Less(i, j int) bool {
	return c[i].year < c[j].year
}
```

구조체 배열 타입의 커스텀 인터페이스를 만들고, 필요한 메소드들을 구현해줍니다.

만약 Len, Swap, Less 메소드를 구현하지 않는다면, 아래와 같은 오류가 출력되면서 implement하라고 알려줍니다.

cannot use customInterface(myBlog) (type customInterface) as type sort.Interface in argument to sort.Sort: customInterface does not implement sort.Interface (missing Len method)

```go
func main() {
	strs := []string{"c", "a", "b"}
	sort.Strings(strs)
	fmt.Println(strs)

	ints := []int{3, 1, 2}
	sort.Ints(ints)
	fmt.Println(ints)

	s := sort.IntsAreSorted(ints)
	fmt.Println(s)
```

타입의 기본형들은 배열에 넣고 오른차순으로 정렬할 수 있게 준비가 되어 있습니다.

그리고 IntsAreSorted로 이미 정렬되어있는지 확인할 수 있습니다.

```go
	myBlog := []blog{
		{"example blog","test", 4},
		{"example blog","test", 3},
		{"example blog","test", 1},
		{"example blog","test", 2},
	}
```

커스텀된 구조로 만들고 싶어서 아까 만든 구조체를 사용해보았습니다.

```go
	fmt.Println(myBlog)
	sort.Sort(customInterface(myBlog))
	fmt.Println(myBlog)
}
```

아까 구현한 Less메소드로 인하여 int형의 맴버 필드를 기준으로 정렬되게 됩니다.