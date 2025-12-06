---
layout: post
title: Golang 페이지 교체 알고리즘 지원하는 캐시 GCache 라이브러리 알아보기
---

오늘은 Golang으로 페이지 교체 알고리즘인 LFU, LRU 그리고 ARC를 지원하는 캐시를 구현한 GCache 라이브러리를 알아보려 합니다.

## GCache 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 GCache 패키지를 설치합니다.

```
go get github.com/bluele/gcache
```

홈의 go 폴더에 GCache 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"time"

	"github.com/bluele/gcache"
)
```

fmt와 time, 그리고 gcache를 가져옵니다.

```go
func main() {
	gc := gcache.New(20).
		LRU().
		Build()
	gc.Set("key", "value")
```

캐시의 사이즈와 페이지 교체 알고리즘을 정하면서 gcache를 생성합니다.

그리고 키와 값을 정해 캐싱합니다.

```go
	value, err := gc.Get("key")
	if err != nil {
		panic(err)
	}
	fmt.Println(value)
```

키로 값을 가져올 수 있습니다.

```go
	gc.SetWithExpire("key", "value", time.Second*3)
	value, err = gc.Get("key")
	if err != nil {
		panic(err)
	}
	fmt.Println("Get:", value)
```

유효 시간을 정해 캐싱할 수 있습니다.

```go
	time.Sleep(time.Second * 5)

	value, err = gc.Get("key")
	if err != nil {
		fmt.Println(err)
	}
```

만약 유효 시간을 정한 뒤에 캐싱하고 잠시 쉬면, 값을 찾을 수 없습니다.

```go
	gc = gcache.New(20).
		LRU().
		LoaderFunc(func(key interface{}) (interface{}, error) {
			return "load", nil
		}).
		Build()
```

캐시 된 값이 만료되면 Loader 함수로 새 값을 생성할 수 있습니다.

```go
	value, err = gc.Get("key")
	if err != nil {
		panic(err)
	}
	fmt.Println(value)
```

Loader 함수로 생성한대로 출력됩니다.

```go
	gc = gcache.New(20).
		LRU().
		LoaderExpireFunc(func(key interface{}) (interface{}, *time.Duration, error) {
			expire := 1 * time.Second
			return "ok", &expire, nil
		}).
		EvictedFunc(func(key, value interface{}) {
			fmt.Println("evicted")
		}).
		PurgeVisitorFunc(func(key, value interface{}) {
			fmt.Println("purged")
		}).
		Build()
```

Loader, Evicted, PurgeVisitor 함수를 만들 수 있습니다.

```go
	value, err = gc.Get("key")
	if err != nil {
		panic(err)
	}
	fmt.Println(value)
```

유효 시간이 지나기 전에는 값이 출력됩니다.

```go
	time.Sleep(1 * time.Second)

	value, err = gc.Get("key")
	if err != nil {
		panic(err)
	}
	fmt.Println(value)
```

유효 시간이 지난 다음에는 Evicted 함수가 수행됩니다.

```go
	gc.Purge()
}
```

마지막으로 Purge를 수행하면 PurgeVisitor 함수가 수행됩니다.