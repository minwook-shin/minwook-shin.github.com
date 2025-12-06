---
layout: post
title: Go 언어 text/scanner 알아보기
---

오늘은 Go 언어 기본 패키지에 포함되있는 text/scanner를 이용하여 스캐너 및 토큰화 해보려 합니다.

```go
package main

import (
	"fmt"
	"strings"
	"text/scanner"
)
```

text/scanner 패키지와 strings 패키지로 텍스트를 스캔하고 토크나이저를 수행할 수 있게 할 수 있습니다.

fmt 패키지로 출력을 담당합니다.

```go
func main() {
	var source = `
// package.
package main

// import
import (
	"fmt"
)

// main
func main() {
	fmt.println("hello, world!")
}`
```

간단한 hello, world를 출력하는 코드 텍스트를 변수에 담았습니다.

```go
	var scannerVar scanner.Scanner
	scannerVar.Init(strings.NewReader(source))
	scannerVar.Filename = "example.go"
```

코드 텍스트를 스캐너에서 토큰화를 위한 준비를 합니다.

파일 이름을 지정할 수도 있습니다.

```go
	for tok := scannerVar.Scan(); tok != scanner.EOF; tok = scannerVar.Scan() {
		fmt.Printf("%s: %s\n", scannerVar.Position, scannerVar.TokenText())
	}
}
```

for문으로 스캐너의 끝까지 반복하면서 토큰화된 텍스트의 위치와 문자를 출력해줍니다.

주석은 스캐너의 모드가 기본인 상태에서는 출력되지 않습니다.