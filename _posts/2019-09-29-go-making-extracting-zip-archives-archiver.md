---
layout: post
title: Golang 압축 파일 만들고 해제하는 Archiver 라이브러리 알아보기
---

오늘은 Golang으로 파일과 폴더들로 압축 파일을 만들고, 해제하여 원본 파일을 조회할 수 있는 Archiver 라이브러리를 알아보려 합니다.

## Archiver 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

맥에서 Golang의 버전을 올리려면 brew를 이용합니다.

```
brew upgrade go
```

우분투에서도 Golang의 버전을 올리려면 backports 저장소를 등록하고 apt를 이용합니다.

```
sudo add-apt-repository ppa:longsleep/golang-backports
sudo apt-get update
sudo apt-get install golang-go
```

go get으로 Archiver 패키지를 설치합니다.

```
go get -u github.com/mholt/archiver/cmd/arc
```

홈의 go 폴더에 Archiver 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"compress/flate"
	"fmt"

	"github.com/mholt/archiver"
)
```

flate, fmt와 archiver를 가져옵니다.

```go
func main() {
	zip := archiver.Zip{
		CompressionLevel:       flate.BestCompression,
		MkdirAll:               true,
		SelectiveCompression:   true,
		ContinueOnError:        false,
		OverwriteExisting:      true,
		ImplicitTopLevelFolder: false,
	}
```

zip 파일로 만들 옵션을 정해서 만들 수 있습니다.

```go
	files := []string{
		"test_src.md",
	}
```

압축할 파일을 정합니다.

```go
	err := zip.Archive(files, "test.zip")
	if err != nil {
		panic(err)
	}
```

해당 파일들을 가지고, zip 파일로 압축합니다.

```go
	err = archiver.Extract("test.zip", "test_src.md", "Extract_output")
	if err != nil {
		panic(err)
	}
```

만든 zip 파일에서 원하는 파일을 추출합니다.

```go
	err = archiver.Unarchive("test.zip", "Unarchive_output")
	if err != nil {
		panic(err)
	}
```

만든 zip 파일로 원하는 위치에 압축 해제합니다.

```go
	err = archiver.Walk("test.zip", func(f archiver.File) error {
		fmt.Println(f.Name(), f.Size())
		return nil
	})
	if err != nil {
		panic(err)
	}
}
```

만든 zip 파일에 있는 파일 목록을 가져와서 이름과 크기를 조회합니다.