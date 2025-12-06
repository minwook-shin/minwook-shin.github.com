---
layout: post
title: Go 언어 collection 함수 알아보기
---

오늘은 Go 언어에서의 콜렉션 함수 구현하는 것을 간단한 코드로 알아보려 합니다.

```go
package main

import (
	"fmt"
	"strings"
)
```

문자열을 조작하기 위한 패키지인 strings과 문자열을 출력하기 위한 패키지인 fmt를 가져옵니다.

고 언어에서 제네릭을 지원하지 않기에 필요한 경우에는 콜렉션 함수를 제공합니다.

```go
func indexFunc(str []string, t string) int {
	for i, j := range str {
		if j == t {
			return i
		}
	}
	return -1
}
```

문자열을 문자열 배열에서 찾아서 해당 문자열과 같은 위치의 인덱스 값을 반환합니다.

for range로 탐색해여 찾을 문자열과 같다면 해당 인덱스를 반환하고, 아니면 -1을 반환하게 됩니다.

```go
func includeFunc(str []string, t string) bool {
	return indexFunc(str, t) >= 0
}
```

문자열이 존재하는 지에 따라 bool 값이 반환됩니다.

구현한 index 함수를 이용하여 -1가 아니면 참을 반환하게 합니다.

```go
func anyFunc(str []string, f func(string) bool) bool {
	for _, j := range str {
		if f(j) {
			return true
		}
	}
	return false
}
```

여러 문자열 중에 하나라도 인자로 받는 함수의 조건에 부합한다면, 참을 반환하고 아니면 거짓을 반환합니다.

```go
func allFunc(str []string, f func(string) bool) bool {
	for _, j := range str {
		if !f(j) {
			return false
		}
	}
	return true
}
```

여러 문자열 모두가 인자로 받는 함수의 조건에 부합한다면, 참을 반환하고 아니면 거짓을 반환합니다.

```go
func filterFunc(str []string, f func(string) bool) []string {
	newArr := []string{}
	for _, j := range str {
		if f(j) {
			newArr = append(newArr, j)
		}
	}
	return newArr
}
```

인자로 받는 함수를 거쳐서 부합하는 문자열만 새롭게 []string를 구성하여 반환합니다.

```go
func mapFunc(str []string, f func(string) string) []string {
	newArr := make([]string, len(str))
	for i, j := range str {
		newArr[i] = f(j)
	}
	return newArr
}
```

인자로 받는 함수를 거쳐서 새롭게 []string를 구성하여 반환합니다.