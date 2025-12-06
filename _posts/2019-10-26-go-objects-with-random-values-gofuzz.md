---
layout: post
title: Golang Fuzz testing gofuzz 라이브러리 알아보기
---

오늘은 Golang으로 무작위 값을 넣어서 테스트하는 Fuzz testing 도구인 gofuzz 라이브러리를 알아보려 합니다.

## gofuzz 설치

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

go get으로 gofuzz 패키지를 설치합니다.

```
go get github.com/google/gofuzz
```

홈의 go 폴더에 gofuzz 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	fuzz "github.com/google/gofuzz"
)
```

fmt와 gofuzz를 가져옵니다.

```go
func main() {
	Fuzzer := fuzz.New()
```

Fuzzer를 반환합니다.

```go
	var intValue int
	Fuzzer.Fuzz(&intValue)
	fmt.Printf("%#v \n", intValue)
```

만든 Fuzzer로 무작위 값을 생성해서 변수에 넣습니다.

```go
	Fuzzer = fuzz.New().NilChance(0).NumElements(1, 1)
```

nil 포인터를 생성할 확률과 최소 및 최대 Elements를 정할 수 있습니다.

```go
	var mapValue map[int]string
	Fuzzer.Fuzz(&mapValue)
	fmt.Printf("%#v \n", mapValue)
```

만든 Fuzzer로 무작위 값을 생성해서 Map에 넣습니다.

```go
	Fuzzer = fuzz.New().NilChance(1)
	var structValue struct {
		A, B *string
	}
	Fuzzer.Fuzz(&structValue)
	fmt.Printf("%#v \n", structValue)
```

무조건 Nil로 채우려면 NilChance(1)를 사용합니다.

```go
	type testStruct struct {
		A, B *string
	}
```

구조체를 만듭니다.

```go
	Fuzzer = fuzz.New().NilChance(0).Funcs(
		func(e *testStruct, c fuzz.Continue) {
			switch c.Intn(2) {
			case 0:
				c.Fuzz(&e.A)
			case 1:
				c.Fuzz(&e.B)
			}
		},
	)
```

커스텀 fuzzing 함수를 만듭니다.

```go
	var testObject testStruct
	Fuzzer.Fuzz(&testObject)
	fmt.Printf("%#v \n", testObject)
}
```

fuzzing 함수로 무작위 값을 생성해서 객체에 넣습니다.