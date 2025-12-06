---
layout: post
title: Golang 키와 값으로 이루어진 디스크 기반의 저장소 Diskv 라이브러리 알아보기
---

오늘은 Golang으로 키와 값을 쌍으로 묶어서 사용하는 디스크 기반의 저장소 시스템인 Diskv 라이브러리를 알아보려 합니다.

## Diskv 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 Diskv 패키지를 설치합니다.

```
go get github.com/peterbourgon/diskv
```

홈의 go 폴더에 Diskv 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/peterbourgon/diskv"
)
```

fmt와 diskv를 가져옵니다.

```go
func main() {
	flatTransform := func(s string) []string { return []string{} }
```

transform 함수로 모든 데이터를 정해둔 폴더에 넣어주게 합니다.

```go
	diskv := diskv.New(diskv.Options{
		BasePath:     "data-dir",
		Transform:    flatTransform,
		CacheSizeMax: 1024 * 1024,
	})
```

BasePath로 폴더의 경로를 정할 수 있으며, 방금 만든 Transform 함수외에도 AdvancedTransform, InverseTransform 함수도 만들어서 사용할 수 있습니다.

그리고 위와 같이 옵션을 설정하면 1MB 용량이 할당됩니다.

```go
	diskv.Write("byteKey", []byte{'h', 'e', 'l', 'l', 'o'})
```

byteKey라는 키와 바이트가 저장됩니다.

```go
	value, err := diskv.Read("byteKey")
	if err != nil {
		panic(err)
	}
	
	fmt.Printf("%v\n", value)
```

불러올 때에는 Read로 키를 사용합니다.

```go
	diskv.WriteString("stringKey", "hello")
	fmt.Println(diskv.ReadString("stringKey"))
```

문자열을 기록하고 읽을 수도 있습니다.

```go
	diskv.Erase("byteKey")
	diskv.Erase("stringKey")
}
```

해당 키의 값들을 삭제합니다.