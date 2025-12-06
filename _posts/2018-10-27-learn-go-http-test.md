---
layout: post
title: Go 언어 net/http/httptest 알아보기
---

오늘은 Go 언어 기본 패키지에 포함되있는 net/http/httptest로 서버를 테스트하는 실습을 해보려 합니다.

```go
package main

import (
	"fmt"
	"io"
	"io/ioutil"
	"net/http"
	"net/http/httptest"
)
```

서버를 구성하기 위해 io와 httptest 패키지를 가져오고, 출력을 위해 fmt 패키지도 가져옵니다.

```go
func main() {
	handler := func(w http.ResponseWriter, r *http.Request) {
		io.WriteString(w, "<html><body>Hello World!</body></html>")
	}
```

버퍼의 끝에 문자열을 기록하기 위해 io 패키지를 가져왔으므로 사용해주고, 핸들러를 구현해줍니다.

```go
	request := httptest.NewRequest("GET","http://example.com", nil)
	responseRecorder := httptest.NewRecorder()
	handler(responseRecorder, request)
```

핸들러에 전달될 서버의 요청과 ResponseRecorder를 생성해줍니다.

```go
	response := responseRecorder.Result()
	bodyByte, _ := ioutil.ReadAll(response.Body)
	fmt.Println(response.StatusCode)
	fmt.Println(http.StatusText(response.StatusCode))
	fmt.Println(string(bodyByte))
}
```

서버의 상태 코드와 페이지의 html 소스를 확인할 수 있습니다.