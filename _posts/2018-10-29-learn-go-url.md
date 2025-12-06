---
layout: post
title: Go 언어 net/url 알아보기
---

오늘은 Go 언어 기본 패키지에 포함되있는 net/url로 URL을 파싱하고 수정하는 실습을 해보려 합니다.

```go
package main

import (
	"encoding/json"
	"fmt"
	"net/url"
)
```

url을 다루기 위한 net/url 패키지와 json 객체를 다루기 위한 encoding/json 패키지를 가져오고, 출력을 위한 fmt 패키지도 같이 가져옵니다.

```go
func main() {
	value := url.Values{}
	value.Set("name", "go")
	value.Add("friend", "c")
	value.Add("friend", "java")
	value.Add("friend", "python")
	fmt.Println(value.Encode())
```

url의 퉈리 스트링에 값을 넣어서 인코딩하여 출력할 수 있습니다.

```go
	parseValue, _ := url.ParseQuery(`name=go&friend=c++&friend=java;ect`)
	jsonByte, _ := json.Marshal(parseValue)
	fmt.Println(string(jsonByte))
```

기존에 작성되있는 쿼리 스트링을 json으로 불러와서 출력할 수 있습니다.

```go
	url, _ := url.Parse("http://golang.org")
	url.Scheme = "https"
	url.Host = "google.com"
	queryValue := url.Query()
	queryValue.Set("q", "golang")
	url.RawQuery = queryValue.Encode()
	fmt.Println(url)
}
```

url을 파싱해서 스키마와 호스트를 원하는 대로 변경하고, 쿼리 스트링도 추가할 수 있습니다.