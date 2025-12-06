---
layout: post
title: Golang trie 구현하는 Trie 라이브러리 알아보기
---

오늘은 Golang으로 단어를 빠르게 찾을 수 있는 트리 구조인 trie를 구현한 Trie 라이브러리를 알아보려 합니다.

## Trie 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 Trie 패키지를 설치합니다.

```
go get github.com/derekparker/trie
```

홈의 go 폴더에 Trie 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/derekparker/trie"
)
```

fmt와 trie를 가져옵니다.

```go
func main() {
	t := trie.New()
```

루트 노드를 가진 trie를 생성합니다.

```go
	t.Add("helloworld", 1)
	t.Add("worldhello", 2)
```

trie에 키를 추가합니다.

```go
	node, ok := t.Find("helloworld")
	meta := node.Meta()
	fmt.Println(meta, ok)
```

키와 일치하는 노드를 반환하여, 메타 정보를 가져옵니다.

```go
	helloResult1 := t.PrefixSearch("hello")
	fmt.Println(helloResult1)
	worldResult1 := t.PrefixSearch("world")
	fmt.Println(worldResult1)
```

접두사로 검색을 할 수 있습니다.

```go
	wrongResult1 := t.PrefixSearch("keyword")
	fmt.Println(wrongResult1)
```

만약 틀리면 빈 슬라이스로 출력됩니다.

```go
	helloResult2 := t.HasKeysWithPrefix("hello")
	fmt.Println(helloResult2)
	worldResult2 := t.HasKeysWithPrefix("world")
	fmt.Println(worldResult2)
```

접두사로 검색하며 bool 값으로 출력됩니다.

```go
	wrongResult2 := t.HasKeysWithPrefix("keyword")
	fmt.Println(wrongResult2)
```

마찬가지로 틀리면 false로 출력됩니다.

```go
	Fuzzyresult1 := t.FuzzySearch("hw")
	fmt.Println(Fuzzyresult1)
	Fuzzyresult2 := t.FuzzySearch("wh")
	fmt.Println(Fuzzyresult2)
}
```

문자열 퍼지 검색을 통해서 검색되는 문자열을 슬라이스로 출력합니다.