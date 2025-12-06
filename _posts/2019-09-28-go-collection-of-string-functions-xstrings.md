---
layout: post
title: Golang 문자열 함수의 모음 xstrings 라이브러리 알아보기
---

오늘은 Golang으로 유용한 문자열 함수들을 모아둔 xstrings 라이브러리를 알아보려 합니다.

## xstrings 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

맥에서 Golang의 버전을 올리려면 brew를 이용합니다.

```
brew upgrade go
```

우분투에서도 Golang의 버전을 올리려면 backports 저장소를 등록하고 apt를 이용합니다.

```
sudo add-apt-repository ppa:longsleep/golang-backports
sudo apt-get update
sudo apt-get install golang-go
```

go get으로 xstrings 패키지를 설치합니다.

```
go get github.com/huandu/xstrings
```

홈의 go 폴더에 xstrings 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/huandu/xstrings"
)
```

fmt와 xstrings를 가져옵니다.

```go
func main() {
	fmt.Println(xstrings.Center("hello", 10, "123"))
```

원하는 길이보다 좁으면 추가적으로 양 옆에 문자열을 붙입니다.

```go
	fmt.Println(xstrings.Count("hello", "aeiou"))
```

정한 패턴에 맞는 문자만 셉니다.

```go
	fmt.Println(xstrings.Delete("hello", "aeiou"))
```

정한 패턴에 맞는 문자를 삭제합니다.

```go
	fmt.Println(xstrings.ExpandTabs("h\tell\to\twor\tld", 4))
```

```\t``` 문자열을 공백으로 확장합니다.

```go
	fmt.Println(xstrings.FirstRuneToLower("HELLO"))
	fmt.Println(xstrings.FirstRuneToUpper("hello"))
```

첫 문자를 대문자/소문자로 바꿉니다.

```go
	fmt.Println(xstrings.Insert("hello", "world", 1))
```

원 문자열에 특정 인덱스로 문자열을 넣습니다.

```go
	fmt.Println(xstrings.Partition("hello", "l"))
	fmt.Println(xstrings.LastPartition("hello", "l"))
```

파티션으로 문자열을 나눕니다.

```go
	fmt.Println(xstrings.LeftJustify("hello", 10, "11"))
	fmt.Println(xstrings.RightJustify("hello", 10, "11"))
```

원하는 길이보다 문자열이 좁으면 오른쪽/왼쪽으로 치우쳐서 채웁니다.

```go
	fmt.Println(xstrings.Len("hello"))
```

문자열의 길이를 측정합니다.

```go
	fmt.Println(xstrings.Reverse("hello"))
```

문자열을 뒤집어 놓습니다.

```go
	fmt.Println(xstrings.RuneWidth('가'))
```

Rune의 문자 Width를 측정합니다.

```go
	fmt.Println(xstrings.Shuffle("hello"))
```

Rune을 무작위로 섞습니다.

```go
	fmt.Println(xstrings.Slice("hello", 1, 3))
```

문자열을 원하는 부분으로 자릅니다.

```go
	fmt.Println(xstrings.Squeeze("hello", "l"))
```

반복적인 문자열을 지웁니다.

```go
	fmt.Println(xstrings.Successor("hello1"))
```

successor 함수를 만들어서 Ruby의 succ 함수처럼 다음 수를 구할 수 있습니다.

```go
	fmt.Println(xstrings.SwapCase("hELLO"))
```

대소문자를 서로 바꿉니다.

```go
	fmt.Println(xstrings.ToCamelCase("hello_world"))
	fmt.Println(xstrings.ToKebabCase("hello_world"))
	fmt.Println(xstrings.ToSnakeCase("HelloWorld"))
```

CamelCase, KebabCase, SnakeCase로 변환합니다.

```go
	fmt.Println(xstrings.Translate("hello", "^l", "*"))
```

원 문자열에 l이 아닌 문자를 원하는 문자로 바꿉니다.

```go
	fmt.Println(xstrings.Width("hello world"))
```

문자열 width를 구합니다.

```go
	fmt.Println(xstrings.WordCount("hello_world"))
```

문자열에서 단어의 수를 셉니다.

```go
	fmt.Println(xstrings.WordSplit("hello_world"))
}
```

문자열에서 단어를 분리하여 리스트로 반환합니다.