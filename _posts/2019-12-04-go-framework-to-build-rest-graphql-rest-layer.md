---
layout: post
title: Golang REST/GraphQL REST Layer 라이브러리 알아보기
---

오늘은 Golang으로 REST/GraphQL 빌드 프레임워크 REST Layer 라이브러리를 알아보려 합니다.

## REST Layer 설치

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

go get으로 REST Layer 패키지를 설치합니다.

```
go get github.com/rs/rest-layer
```

홈의 go 폴더에 REST Layer 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"log"
	"net/http"

	"github.com/rs/rest-layer/resource"
	"github.com/rs/rest-layer/resource/testing/mem"
	"github.com/rs/rest-layer/rest"
	"github.com/rs/rest-layer/schema"
)
```

log, net 그리고 rest-layer 패키지를 가져옵니다.

```go
var (
	test = schema.Schema{
		Description: `test object`,
		Fields: schema.Fields{
			"id": {
				Required:   true,
				ReadOnly:   true,
				Filterable: true,
				Sortable:   true,
				OnInit:     schema.NewID,
				Validator: &schema.String{
					Regexp: "^[0-9a-z]$",
				},
			},
```

test 리소스의 스키마를 만들고, id 필드를 정의합니다.

필수 여부, read-only로 지정하고, 정렬하거나 필터링할 수도 있게 할 수 있습니다.

그리고 필드에 맞는 hook과 Validator를 준비할 수 있습니다.

```go
			"created": {
				Required:   true,
				ReadOnly:   true,
				Filterable: true,
				Sortable:   true,
				OnInit:     schema.Now,
				Validator:  &schema.Time{},
			},
		},
	}
)
```

created 필드도 정의합니다.

이 필드는 Validator를 다르게 설정합니다.

```go
func main() {
	index := resource.NewIndex()
```

REST API 리소스 인덱스를 생성합니다.

```go
	index.Bind("tests", test, mem.NewHandler(), resource.Conf{AllowedModes: resource.ReadWrite})
```

tests 라는 엔드포인트를 가진 리소스를 바인딩합니다.

```go
	api, _ := rest.NewHandler(index)
```

API HTTP 핸들러를 생성합니다.

```go
	http.Handle("/api/", http.StripPrefix("/api/", api))
```

/api 경로를 추가합니다.

```go
	if err := http.ListenAndServe(":8080", nil); err != nil {
		log.Fatal(err)
	}
}
```

8080 포트로 서버를 구동합니다.