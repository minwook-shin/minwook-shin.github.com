---
layout: post
title: Golang 인 메모리 캐시 cache2go 라이브러리 알아보기
---

오늘은 Golang으로 타임아웃을 지원하는 인 메모리 캐시 cache2go 라이브러리를 알아보려 합니다.

## cache2go 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 cache2go 패키지를 설치합니다.

```
go get github.com/muesli/cache2go
```

홈의 go 폴더에 cache2go 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"time"

	"github.com/muesli/cache2go"
)
```

fmt와 time, 그리고 cache2go를 가져옵니다.

```go
type customStruct struct {
	text  string
	extra []byte
}
```

구조체를 만들어서 임의의 구조의 값을 캐시로 저장할 수 있습니다.

```go
func main() {
	cache := cache2go.Cache("cacheTable")
```

캐시 테이블을 생성합니다.

```go
	val := customStruct{"text", []byte{'a'}}
	cache.Add("TestKey", 2*time.Second, &val)
```

임의로 만든 구조체로 값을 채운뒤에 키와 값을 캐시에 추가합니다.

이 때에 캐시에 남아있을 유효 시간을 설정할 수 있습니다.

```go
	// cache.Add("TestKey", 0, &val)
```

만약 무한으로 두고 싶으면 0으로 설정할 수 있습니다.

```go
	res, err := cache.Value("TestKey")
	if err == nil {
		text := res.Data().(*customStruct).text
		extra := res.Data().(*customStruct).extra
		fmt.Println(text, extra)
	} else {
		panic(err)
	}
```

키를 이용하여 캐시 테이블에서 값을 가져옵니다.

```go
	time.Sleep(3 * time.Second)
```

시간을 지연시켜서 캐시의 유효 시간을 지나게 합니다.

```go
	res, err = cache.Value("TestKey")
	if err != nil {
		fmt.Println("not cached.")
	}
```

다시 키로 값을 가져올 때에 캐시에 저장된 값이 없음을 알 수 있습니다.

```go
	cache.Add("TestKey", 0, &val)
```

테스트를 위해 다시 키와 값을 만듭니다.

```go
	cache.SetAboutToDeleteItemCallback(func(e *cache2go.CacheItem) {
		key := e.Key()
		TextData := e.Data().(*customStruct).text
		ExtraData := e.Data().(*customStruct).extra
		created := e.CreatedOn()
		accessed := e.AccessedOn()
		fmt.Println(key, TextData, ExtraData, created, accessed)
	})
```

콜백에 의해 캐시에서 아이탬이 지워질 때마다 어떤 값이 지워졌는지 알 수 있습니다.

```go
	cache.Delete("TestKey")
```

키로 삭제하면 위 콜백이 수행됩니다.

```go
	cache.Flush()
}
```

캐시 테이블을 초기화 합니다.