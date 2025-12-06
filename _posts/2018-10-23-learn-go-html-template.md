---
layout: post
title: Go 언어 template 알아보기
---

오늘은 Go 언어 기본 패키지에 포함되있는 html/template을 이용하여 실습해보려 합니다.

```go
package main

import (
	"html/template"
	"os"
)
```

오늘 포스트에 필요한 html/template 패키지와 출력에 필요한 os 패키지를 가져옵니다.

```go
func main() {
	var source = `
	<!DOCTYPE html>
	<html>
		<head>
			<meta charset="UTF-8">
			<title>{{.Title}}</title>
		</head>
		<body>
			{{range .Items}}<div>{{ . }}</div>{{end}}
		</body>
	</html>`
```

html 코드를 문자열로 저장합니다.

향후 구조체를 넣으면 값을 출력해야 되기 때문에 ```{{.Title}}``` 과 같이 값을 넣을 공간을 템플릿으로 만들어줍니다.

만약 for range문 처럼 모든 내용을 출력하고 싶으면 ```range .Items```으로 시작해서 반복할 태그를 작성하고, ```end```로 닫아주면 됩니다.

```go
	webTemplate, _ := template.New("webpage").Parse(source)
```

작성한 html 코드들을 Parse 해줍니다.

```go
	structData := struct {
		Title string
		Body  []string
	}{
		Title: "title",
		Body: []string{
			"list_1",
			"list_2",
		},
	}
	webTemplate.Execute(os.Stdout, structData)
}
```

익명 구조체를 이용하여 값들을 넣어주면서 출력해줍니다.