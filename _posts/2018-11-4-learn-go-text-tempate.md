---
layout: post
title: Go 언어 text/template 알아보기
---

오늘은 Go 언어 기본 패키지에 포함되있는 text/template를 이용하여 간단한 템플릿을 만들어보려 합니다.

```go
package main

import (
	"os"
	"text/template"
)
```

text/template 패키지로 템플릿을 만들어 원하는 문자들을 넣을 수 있습니다.

os 패키지로 표준 출력을 만들 수 있습니다.

```go
func main() {
	var letter = `
{{.Name}}님께,
{{if .Option}}
이 텍스트는 선택된 사람들만 보입니다.
{{- else}}
안녕하세요.
{{- end}}
{{with .Gift -}}
{{.}}를 구입해주셔서 감사합니다.
{{end}}
감사합니다,
gopher.
`
```

템플릿을 만들어서 변수에 넣어줍니다.

Name이라는 구조체의 맴버 필드에 값을 넣으면 {{.Name}}에 원하는 문자가 나타나고, if else를 이용하여 true와 false로 원하는 문자열을 출력하게 할 수 있습니다.

```go
	type textTemplate struct {
        Name string
        Gift string
		Option   bool
	}
	var auto = textTemplate{
		"익명의 구매자", 
		"banana computer", 
		true}
```

구조체를 만들어서 맴버 필드에 값을 넣어줍니다.

```go
	t := template.Must(template.New("letter").Parse(letter))
	t.Execute(os.Stdout, auto)
}
```

만들어둔 템플릿을 파싱하여 구조체 값을 넣어주고 표준 출력을 해봅니다.