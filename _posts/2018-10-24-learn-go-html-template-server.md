---
layout: post
title: Go 언어 template 서버 만들어 보기
---

오늘은 Go 언어 기본 패키지에 포함되있는 html/template을 이용하여 웹 서버를 만드는 실습을 해보려 합니다.

```go
package main

import (
	"flag"
	"html/template"
	"net/http"
)
```

커맨드라인 플래그를 파싱해주는 flag와 템플릿을 만들어주는 html/template 패키지를 가져옵니다.

그리고 웹 서버에 만들 net/http 패키지도 가져옵니다.

```go
var templateStr = `
<html>
<head>
<title>hello world</title>
</head>
<body>
<p>hello world!</p>
</body>
</html>
`
```

간단한 html 코드를 문자열로 저장합니다.

```go
var address = flag.String("addr", ":8080", "http service address")
var templateParser = template.Must(template.New("").Parse(templateStr))
```

addr라는 이름의 플래그에 :8080를 기본으로 달아줍니다.

그리고 위 html 코드를 파싱해줍니다.

```go
func main() {
	flag.Parse()
	http.Handle("/", http.HandlerFunc(func (w http.ResponseWriter, req *http.Request)  {
		templateParser.Execute(w, req)
	}))
```

이제 메인 함수에서는 위에서 선언한 플래그를 실핼하기 위해 Parse() 명령을 수행합니다.

그리고 http 핸들러를 지정해줍니다.
루트 디렉토리에 접속하면 미리 만들어둔 html 코드를 파싱한 템플릿을 웹 서버에 뿌려주는 익명 함수가 HandlerFunc에 의해 실행되게 됩니다.

```go
	http.ListenAndServe(*address, nil)
}
```

아까 만든 플래그로 포트를 지정하여 웹 서버를 시작하고, 고투린에 작업을 맡깁니다.

