---
layout: post
title: Go 언어 zip 파일 압축 배워보기 
---

오늘은 Go 언어 기본 패키지에 포함되있는 archive/zip을 이용하여 압축파일을 생성해보려 합니다.


```go
package main

import (
	"archive/zip"
	"path/filepath"
	"io"
	"os"
)
```

우선 archive/zip 패키지와 파일 경로를 탐색하기 위해 path/filepath 패키지도 가져옵니다.

io와 os 패키지로 파일을 핸들링할 예정이므로 이 두 패키지도 가져와줍니다.

```go
func main() {
	file, _ := os.Create("./test.zip")
	defer file.Close()
	archive := zip.NewWriter(file)
	defer archive.Close()
```

파일과 zip writer 객체도 생성해주고, defer 키워드로 작업이 끝나면 닫아주게 예약해둡니다.

```go
	filepath.Walk("./db.csv", 
	func(path string, name os.FileInfo, _ error) error {
```

Walk 함수로 주어진 내부의 모든 경로 트리를 순회하도록 하면서, 오류 값을 반환으로 하는 익명의 WalkFunc 함수도 같이 인자로 받습니다.

```go
		header, _ := zip.FileInfoHeader(name)
		header.Method = zip.Deflate
		writer, _ := archive.CreateHeader(header)
```

zip 파일의 헤더를 작성해줍니다.

```go
		file, error := os.Open(path)
		defer file.Close()
		io.Copy(writer, file)
		return error
	})
}
```

마지막으로 해당 파일의 경로를 열고 io 패키지를 이용하여 writer로 복사해줍니다.