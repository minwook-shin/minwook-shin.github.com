---
layout: post
title: Golang C# LINQ 구현한 go-linq 라이브러리 알아보기
---

오늘은 Golang으로 C# 언어에서 존재하는 직접 쿼리 기능을 통합하는 방식인 LINQ를 구현한 go-linq 라이브러리를 알아보려 합니다.

## go-linq 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 go-linq 패키지를 설치합니다.

```
go get gopkg.in/ahmetb/go-linq.v3
```

홈의 go 폴더에 go-linq 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"strings"

	. "github.com/ahmetb/go-linq"
)
```

fmt, strings 그리고 go-linq 패키지를 가져옵니다.

```go
func main() {
	type Test struct {
		ID    int
		Email string
		Names []string
	}
```

테스트에 필요한 구조체를 만듭니다.

```go
	TestIds := []Test{}
	n := Test{ID: 1001, Email: "test@example.com", Names: []string{"name_1", "sub_name_1"}}
	TestIds = append(TestIds, n)
	n = Test{ID: 999, Email: "test2@example.com", Names: []string{"name_2"}}
	TestIds = append(TestIds, n)
```

해당 구조체의 형식을 맞춘 객체들의 리스트를 만들어줍니다.

```go
	var output1 []string
	From(TestIds).Where(func(c interface{}) bool {
		return c.(Test).ID <= 1000
	}).Select(func(c interface{}) interface{} {
		return c.(Test).Email
	}).ToSlice(&output1)
	fmt.Println(output1)
```

From, Where, Select 문법을 이용하여 범위와 조건을 지정하고 선택하여 출력합니다.

ID 값이 1000보다 작은 객체만 골라져서 슬라이스로 출력하게 됩니다.

```go
	output2 := From(TestIds).SelectMany(
		func(test interface{}) Query {
			return From(test.(Test).Names)
		}).GroupBy(
		func(name interface{}) interface{} {
			return name
		}, func(name interface{}) interface{} {
			return name
		}).OrderByDescending(
		func(group interface{}) interface{} {
			return len(group.(Group).Group)
		}).Select(
		func(group interface{}) interface{} {
			return group.(Group)
		}).First()
	fmt.Println(output2)
```

SelectMany, GroupBy, OrderByDescending, Select 문법을 이용하여 집합하고 그룹으로 묶어서 순서를 정렬한 뒤에 첫번째 내용만 보이게 할 수 있습니다.

```go
	var output3 []string
	testSlice := []string{"a bb", "a ccc dddd eeeee ffffff"}
	From(testSlice).
		SelectManyT(func(testSlice string) Query {
			return From(strings.Split(testSlice, " "))
		}).
		GroupByT(
			func(word string) string { return word },
			func(word string) string { return word },
		).
		OrderByDescendingT(func(wordGroup Group) int {
			return len(wordGroup.Group)
		}).
		ThenByT(func(wordGroup Group) string {
			return wordGroup.Key.(string)
		}).
		Take(5).
		SelectT(func(wordGroup Group) string {
			return fmt.Sprintf("[%s / %d]", wordGroup.Key, len(wordGroup.Group))
		}).
		ToSlice(&output3)
	fmt.Println(output3)
}
```

SelectManyT, GroupByT, OrderByDescendingT, ThenByT, Take, SelectT, ToSlice 를 이용하여 문자열을 분리하고 서로 같은 단어끼리 묶은 뒤에 정렬해서 5개까지만 슬라이스로 출력합니다.

단, T 접미사 메소드는 Generic 함수처럼 interface를 계속 정의하지 않아도 됩니다.