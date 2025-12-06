---
layout: post
title: Golang 키와 값을 저장하거나 캐싱할 수 있는 go-cache 라이브러리 알아보기
---

오늘은 Golang으로 키와 값을 파일로 저장하고 불러오거나, 캐싱할 수 있는 GCache 라이브러리를 알아보려 합니다.

## go-cache 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 go-cache 패키지를 설치합니다.

```
go get github.com/patrickmn/go-cache
```

홈의 go 폴더에 go-cache 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"time"

	"github.com/patrickmn/go-cache"
)
```

fmt와 time 그리고 go-cache를 가져옵니다.

```go
func main() {
	c := cache.New(1*time.Minute, 2*time.Minute)
```

인자에 의해 만료 기간과 정리 간격으로 설정된 캐시를 생성합니다.

```go
	c.Set("test_key_1", "v1", cache.DefaultExpiration)
```

캐시를 만들 때에 기본으로 설정된 만료 시간을 기준으로 키와 값을 캐시에 기록합니다.

```go
	c.Set("test_key_2", 1, cache.NoExpiration)
```

만료하지 않게 할 수도 있습니다.

```go
	c.Delete("test_key_2")
```

해당 키와 값을 캐시에서 삭제합니다.

```go
	getInterface, foundBool := c.Get("test_key_1")
	if foundBool {
		fmt.Println(getInterface)
	}

	if getInterface, foundBool = c.Get("test_key_1"); foundBool {
		value := getInterface.(string)
		fmt.Println(value)
	}
```

타입 assertion을 할 수 있습니다.

```go
	c.SaveFile("test")

	c.Delete("test_key_1")

	c.LoadFile("test")
```

파일로 저장하고 캐시에서 값을 제거해도, 다시 불러오면 값이 존재하는 것을 볼 수 있습니다.

```go
	c.Flush()
}
```

캐시에서 모든 아이템을 지웁니다.