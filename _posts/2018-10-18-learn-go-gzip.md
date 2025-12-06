---
layout: post
title: Go 언어 gzip 알아보기
---

오늘은 Go 언어 기본 패키지에 포함되있는 compress/gzip을 이용하여 실습해보려 합니다.

```go
package main

import (
	"bytes"
	"compress/gzip"
	"io"
	"os"
)
```

우선 버퍼를 만들기 위한 bytes 패키지와 오늘의 포스팅 주제에 필요한 compress/gzip 패키지를 가져옵니다.

내용 출력을 위한 io 패키지와 os 패키지도 가져와줍니다.

```go
func main() {
	var buffer bytes.Buffer
	gzipWriter := gzip.NewWriter(&buffer)
```

버퍼를 만들어주고, 그 버퍼를 이용하여 gzip을 실습하기 위한 writer를 만들어줍니다.

```go
	gzipWriter.Write([]byte("test text"))
	gzipWriter.Close()
```

바이트 배열로 값을 기입해주고, 닫아줍니다.

```go
	gzipReader, _ := gzip.NewReader(&buffer)
	defer gzipReader.Close()
```

버퍼로 Reader를 만들어주고 defer 키워드를 이용해 해당 작업이 끝나면 gzipReader를 닫아줍니다.

```go
	io.Copy(os.Stdout, gzipReader)
}
```

io 패키지의 Copy로 Reader를 writer로 복사하여 값을 출력합니다.