---
layout: post
title: Go 언어 zip 파일 해제 배워보기 
---

오늘은 Go 언어 기본 패키지에 포함되있는 archive/zip을 이용하여 압축파일을 열어보려 합니다.

```go
package main

import (
	"archive/zip"
	"fmt"
	"io"
	"os"
)
```

우선 archive/zip을 이용해야 하므로 이 패키지를 가져오고, 문자열을 출력하기 위한 fmt와 파일 내용을 보기 위해 io, os도 가져옵니다.

```go
func main() {
	r, _ := zip.OpenReader("./test.zip")
	defer r.Close()
```

zip의 OpenReader로 해당 경로의 파일을 오픈해주고, defer 키워드로 해당 작업이 끝날 때에 닫아줍니다.

```go
	for _, f := range r.File {
        fmt.Printf("%s:\n", f.Name)
```

For문으로 각각의 파일들의 이름을 조회할 수 있습니다.

```go
		rc, _ := f.Open()
		io.CopyN(os.Stdout, rc, 10000)
		defer rc.Close()
	}
}
```

파일을 열고 CopyN으로 원하는 바이트 만큼 읽어옵니다.

마지막으로 해당 작업이 끝나면 닫도록 defer 키워드로 예약을 해둡니다.