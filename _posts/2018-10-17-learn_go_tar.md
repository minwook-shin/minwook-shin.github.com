---
layout: post
title: Go 언어 tar 파일 압축과 해제 배워보기 
---

오늘은 Go 언어 기본 패키지에 포함되있는 archive/tar을 이용하여 실습해보려 합니다.

```go
package main

import (
	"archive/tar"
	"bytes"
	"io"
	"os"
)
```

가장 기본이 되는 archive/tar 패키지를 가져오고, 버퍼를 만들기 위해 bytes패키지도 가져옵니다.

io와 os패키지도 터미널에 출력하기 위해 가져옵니다.

```go
func main() {
	var buffer bytes.Buffer
	tarWriter := tar.NewWriter(&buffer)
	defer tarWriter.Close()
```

버퍼를 만들어서 Writer를 생성하기 위한 준비를 합니다.

defer 키워드로 Writer의 일이 끝날 때에 닫도록 작성합니다.

```go
	var files = []struct {
		Name, Body string
	}{
		{"test.txt", "test file"},
	}
```

파일을 익명 구조체로 구성하여 내용을 채워줍니다.

```go
	for _, file := range files {
		tarHeader := &tar.Header{
			Name: file.Name,
			Mode: 0600,
			Size: int64(len(file.Body)),
		}
		tarWriter.WriteHeader(tarHeader)
		tarWriter.Write([]byte(file.Body))
	}
```

for문으로 순회하면서 tar의 헤더를 작성해줍니다.

```go
	tarReader := tar.NewReader(&buffer)
	for {
		io.Copy(os.Stdout, tarReader)
		_, err := tarReader.Next()
		if err == io.EOF {
			break
		}
	}
}
```

이전에 만들어둔 버퍼로 Reader를 생성하여 출력합니다.

간단한 예외처리로 버퍼의 끝이 보이면 멈추도록 합니다.