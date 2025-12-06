---
layout: post
title: Golang 확률적 자료구조 구현하는 Boom Filters 라이브러리 알아보기
---

오늘은 Golang으로 bloom filter, minhash와 같이 확률적으로 원소가 집합에 포함되었는지 검사하거나, 차원을 축소할 수 있는 Boom Filters 라이브러리를 알아보려 합니다.

## Boom Filters 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 Boom Filters 패키지를 설치합니다.

```
go get github.com/tylertreat/BoomFilters
```

홈의 go 폴더에 Boom Filters 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	boom "github.com/tylertreat/BoomFilters"
)
```

fmt와 BoomFilters를 가져옵니다.

```go
func main() {
	stableBloomFilter := boom.NewDefaultStableBloomFilter(10000, 0.01)
```

Stable BloomFilter를 생성할 수 있습니다.

```go
	point := stableBloomFilter.StablePoint()
	fmt.Println(point)
```

StablePoint로 Stable BloomFilter가 얼마나 안정적인지 나타냅니다.

```go
	stableBloomFilter.Add([]byte(`a`))
```

Stable BloomFilter에 데이터를 추가합니다.

```go
	if stableBloomFilter.Test([]byte(`a`)) {
		fmt.Println("A")
	}
```

a라는 데이터가 존재하는지 검사합니다.

이 때에 확률적으로 있을 수 있지만, 없으면 확실하게 없습니다.

```go
	if stableBloomFilter.TestAndAdd([]byte(`b`)) {
		fmt.Println("B")
	} else {
		fmt.Println("not B")
	}
```

먼저 검사하고, 참과 거짓 중에 하나를 반환한 다음에 해당 바이트를 추가합니다.

```go
	if stableBloomFilter.Test([]byte(`b`)) {
		fmt.Println("B")
	}
```

b가 있으므로 이 때에는 true로 반환됩니다.

```go
	stableBloomFilter.Reset()
```

Stable BloomFilter를 원래 상태로 초기화 합니다.

```go
	first := []string{"hello", "world", "c++", "java", "python", "rust", "golang"}
	second := []string{"hello", "world", "c++", "java", "python"}
```

두 개의 string 타입의 슬라이스를 만듭니다.

```go
	similarity := boom.MinHash(first, second)
	fmt.Println(similarity)
}
```

MinHash로 두 문서의 유사성을 추정할 수 있습니다.

그 외에도 해당 라이브러리를 이용하면 Scalable Bloom Filters, Counting Bloom Filters, Inverse Bloom Filters 그리고 Cuckoo Filters, HyperLogLog, Count-Min Sketch와 같이 확률적 자료구조로 코드를 작성할 수 있습니다.