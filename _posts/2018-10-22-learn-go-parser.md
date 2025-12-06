---
layout: post
title: Go 언어 parser 알아보기
---

오늘은 Go 언어 기본 패키지에 포함되있는 go/parser를 이용하여 실습해보려 합니다.

```go
package main

import (
	"go/parser"
    "go/token"
    "fmt"
)
```

이번 포스트에서 필요한 go/parser 패키지와 go/token 패키지를 준비해줍니다.

출력을 위한 fmt 패키지도 준비해줍니다.

```go
func main() {
	fileSet := token.NewFileSet()
```

token 패키지를 통해 파일셋을 생성해줍니다.

```go
	source := 
	`
	package main

	import (
		"fmt"
	)
	
	func main() {
		fmt.Println("hello, world!")
	}
	`
```

go언어 파일을 가져온 것 같은 효과로 문자열을 만들어줍니다.

```go
	file, _ := parser.ParseFile(fileSet, "", source, parser.ImportsOnly)
```

parser 패키지의 ParseFile로 파일셋과 코드 문자열, 파서의 모드를 넣어줍니다.

```go
	fmt.Println(file.Name)
	for _, spec := range file.Imports {
		fmt.Println(spec.Path.Value)
	}
}
```

출력해보면 해당 패키지 이름이 출력되고 이어서 import된 패키지의 이름들도 for문으로 반복하면서 출력됩니다.