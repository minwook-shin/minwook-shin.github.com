---
layout: post
title: Golang GraphQL 서버 graphql-go 라이브러리 알아보기
---

오늘은 Golang으로 GraphQL 서버를 구현한 graphql-go 라이브러리를 알아보려 합니다.

## graphql-go 설치

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

go get으로 graphql-go 패키지를 설치합니다.

```
go get github.com/graph-gophers/graphql-go
```

홈의 go 폴더에 graphql-go 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"net/http"

	graphql "github.com/graph-gophers/graphql-go"
	"github.com/graph-gophers/graphql-go/relay"
)
```

http와 graphql-go를 가져옵니다.

```go
type query struct{}

func (q *query) Search() string {
	return "VALUE"
}
```

query 구조체와 이에 해당하는 메소드를 만듭니다.

```go
func main() {
	s := `
                schema {
                        query: Query
                }
                type Query {
                        search: String!
                }
        `
	schema := graphql.MustParseSchema(s, &query{})
```

스키마를 파싱할 때에 오류 발생하면 패닉을 일으킵니다.

```go
	http.Handle("/",
		&relay.Handler{
			Schema: schema},
	)
```

query 구조체가 포함된 스키마를 DefaultServeMux로 등록합니다.

```go
	err := http.ListenAndServe(":8080", nil)
	if err != nil {
		panic(err)
	}
}
```

Serve를 호출하면서 연결의 요청을 처리합니다.

{"query": "{ search }"}를 POST로 보내면 {"data":{"search":"VALUE"}}으로 반환됩니다.