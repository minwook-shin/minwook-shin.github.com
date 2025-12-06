---
layout: post
title: Go 언어 zlib 알아보기
---

오늘은 Go 언어 기본 패키지에 포함되있는 compress/zlib을 이용하여 실습해보려 합니다.

```go
package main

import (
	"bytes"
	"compress/zlib"
	"fmt"
	"io"
	"os"
)
```

우선 버퍼를 만들기 위한 bytes 패키지와 오늘의 포스팅 주제에 필요한 compress/zlib 패키지를 가져옵니다.

내용 출력을 위한 io 패키지와 os 패키지도 가져와줍니다.

```go
func main() {
	var buffer bytes.Buffer
	writer := zlib.NewWriter(&buffer)
	writer.Write([]byte("test"))
	writer.Close()
```

버퍼를 만들어주고 그 버퍼로 writer를 생성해줍니다.

바이트 배열로 문자열을 Writer에 기록해줍니다.

그리고 작업을 끝냈으면 반드시 닫아줍니다.

```go
	fmt.Println(buffer.Bytes())
```

fmt패키지의 Println으로 buffer에 있는 바이트들을 출력해봅니다.

```go
	reader, _ := zlib.NewReader(&buffer)
    defer reader.Close()
```

아까 생성한 버퍼로 reader를 생성해서 defer 키워드로 main이 끝나기전에 닫게 해줍니다.

```go
	io.Copy(os.Stdout, reader)
}
```

Reader를 writer로 복사하여 io 패키지의 Copy로 출력해줍니다.