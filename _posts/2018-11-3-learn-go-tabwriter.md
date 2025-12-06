---
layout: post
title: Go 언어 text/tabwriter 알아보기
---

오늘은 Go 언어 기본 패키지에 포함되있는 text/tabwriter를 이용하여 탭이 있는 문자열을 정리해보려 합니다.

```go
package main

import (
	"fmt"
	"os"
	"text/tabwriter"
)
```

text/tabwriter 패키지로 탭이 포함된 문자열을 정렬된 텍스트로 변환해줄 수 있습니다.

os 패키지는 라이터를 만들 때에 표준 출력으로 넣을 때 필요하며, fmt 패키지로 출력을 사용합니다.

```go
func main() {
	w := tabwriter.NewWriter(os.Stdout, 0, 0, 0, '*', tabwriter.Debug)
	fmt.Fprintln(w, "a\tb\tc")
	fmt.Fprintln(w, "aa\tbb\tcc")
	fmt.Fprintln(w, "aaa\t\t")
	fmt.Fprintln(w, "aaaa\tbbbb\tcccc")
	w.Flush()
)
```

라이터를 생성하여 공백에 넣을 문자를 넣으면 기본적으로 최대 폭에 맞추어 문자들이 분리됩니다.

```go
	w = tabwriter.NewWriter(os.Stdout, 0, 0, 5, '*', tabwriter.AlignRight|tabwriter.Debug)
	fmt.Fprintln(w, "a\tb\tc")
	fmt.Fprintln(w, "aa\tbb\tcc")
	fmt.Fprintln(w, "aaa\t\t")
	fmt.Fprintln(w, "aaaa\tbbbb\tcccc")
	w.Flush()
}
```

만약 padding 인자에 값을 넣게되면 최대 폭에 더해져서 공백에 문자들이 채워져서 출력됩니다.

플래그를 AlignRight라고 준다면 오른쪽 정렬이 되어서 나타납니다.