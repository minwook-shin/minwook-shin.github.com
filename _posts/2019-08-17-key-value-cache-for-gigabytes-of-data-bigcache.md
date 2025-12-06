---
layout: post
title: Golang 인 메모리 캐시 BigCache 라이브러리 알아보기
---

오늘은 Golang으로 대용량의 데이터를 유지할 수 있는 인 메모리 캐시를 구현한 BigCache 라이브러리를 알아보려 합니다.

## BigCache 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 BigCache 패키지를 설치합니다.

```
go get github.com/allegro/bigcache
```

홈의 go 폴더에 BigCache 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"time"

	"github.com/allegro/bigcache"
)
```

fmt와 time 그리고 bigcache를 가져옵니다.

```go
func main() {
	defaultCache, err := bigcache.NewBigCache(bigcache.DefaultConfig(10 * time.Minute))
	if err != nil {
		panic(err)
	}
```

기본 설정 값으로 BigCache 객체를 생성합니다.

```go
	defaultCache.Set("test_id", []byte("hello"))
```

키와 값을 저장합니다.

```go
	if entry, err := defaultCache.Get("test_id"); err == nil {
		fmt.Println(string(entry))
	}
```

키를 이용하여 값을 가져옵니다.

```go
	config := bigcache.Config{
		Shards:             1024,
		LifeWindow:         10 * time.Minute,
		CleanWindow:        1 * time.Minute,
		MaxEntriesInWindow: 1000 * 10 * 60,
		MaxEntrySize:       500,
		Verbose:            true,
		HardMaxCacheSize:   0,
		OnRemove:           nil,
		OnRemoveWithReason: nil,
	}
```

캐시 샤드, 유지 시간, 만료 제거 간격, 최대 항목 수와 크기 같은 값들과 콜백들을 설정할 수 있습니다.

```go
	customCache, err := bigcache.NewBigCache(config)
	if err != nil {
		panic(err)
	}
```

위에서 설정한 설정 값으로 BigCache 객체를 생성합니다.

```go
	customCache.Set("test_id", []byte("hello"))
```

키와 값을 저장합니다.

```go
	if entry, err := customCache.Get("test_id"); err == nil {
		fmt.Println(string(entry))
	}
}
```

키를 이용하여 값을 가져옵니다.