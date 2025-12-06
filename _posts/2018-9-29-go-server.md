---
layout: post
title: Go 언어 서버 대하여 배워보기 
---

오늘은 Go 언어에서 간단하게 서버를 만들어 보는 내용을  작성하면서 배워보려 합니다.

```go
package main

import (
	"net/http"
)

func hello(w http.ResponseWriter, r *http.Request) {
	w.Write([]byte("Hello World"))
}

func main() {
	http.HandleFunc("/", hello)
	http.ListenAndServe(":8080", nil)
}	
```

hello라는 함수로 웹페이지에 텍스트를 출력하게 해주며, 이 함수의 인자로 인하여 http 문서에 무언가를 쓰거나 요청을 처리할 수 있습니다.

메인 함수에서는 요청할 웹 경로에 핸들러를 배치할 수 있는 HandleFunc 함수가 있습니다.

마지막으로 ListenAndServe 함수는 지정한 포트로 고루틴을 만들어서 서버 작업을 할당해주는 역할을 합니다.

```go
package main
 
import (
    "net/http"
)
 
func main(){
    http.Handle("/", http.FileServer(http.Dir("www")))
    http.ListenAndServe(":8000", nil)
}
```

go언어로 쉽게 파일서버도 만들수 있습니다.

FileServer 함수로 원하는 디렉토리를 정해서 그 안에 존재하는 파일들을 바로 전송해줍니다.
