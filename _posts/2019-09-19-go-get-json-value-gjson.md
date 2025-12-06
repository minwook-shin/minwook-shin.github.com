---
layout: post
title: Golang json 문자열 파싱하는 GJSON 라이브러리 알아보기
---

오늘은 Golang으로 SVG를 생성할 수 있는 GJSON 라이브러리를 알아보려 합니다.

## GJSON 설치

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

go get으로 GJSON 패키지를 설치합니다.

```
go get -u github.com/tidwall/gjson
```

홈의 go 폴더에 GJSON 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"strings"

	"github.com/tidwall/gjson"
)
```

fmt와 strings 그리고 gjson을 가져옵니다.

```go
const json = `{"map":{"first":"hello","last":"world"},
"list": ["first","second","third"],
"int":10}`
```

json 문자열을 만들어둡니다.

```go
func main() {
	gjson.AddModifier("custom", func(json, arg string) string {
		if arg == "upper" {
			return strings.ToUpper(json)
		}
		if arg == "lower" {
			return strings.ToLower(json)
		}
		return json
	})

	result := gjson.Get(json, "map.last")
	fmt.Println(result.String())

	result = gjson.Get(json, "map.last|@custom:upper")
	fmt.Println(result.String())
```

modifiers를 직접 커스텀할 수 있습니다.

직접만든 modifiers로 원하는 결과를 가져올 수 있습니다.

```go
	gjson.ForEachLine(json, func(line gjson.Result) bool {
		fmt.Println(line.String())
		return true
	})
```

여러 줄의 json 문자열을 iterate하게 가져옵니다.

```go
	result = gjson.Get(json, "list")
	for _, name := range result.Array() {
		fmt.Println(name.String())
	}
```

Array로 가져올 수 있습니다.

```go
	result = gjson.Get(json, "map")
	result.ForEach(func(key, value gjson.Result) bool {
		fmt.Println(value.String())
		return true
	})
```

key, value로 나누어서 반복적으로 가져옵니다.

```go
	fmt.Println(gjson.Parse(json).Get("map").Get("last"))
```

간단하게 한 줄로 json 문자열을 파싱해서 원하는 부분만 가져올 수 있습니다.

```go
	result = gjson.Get(json, "map.last")
	if !result.Exists() {
		panic("no last map")
	}
```

값이 존재하는지 확인할 수 있습니다.

```go
	if !gjson.Valid(json) {
		panic("invalid json")
	}
```

json 파일인지 확인할 수 있습니다.

```go
	content, ok := gjson.Parse(json).Value().(map[string]interface{})
	if !ok {
		panic("err")
	}
	fmt.Println(content)
}
```

map으로 만들어서 내용을 출력할 수 있습니다.