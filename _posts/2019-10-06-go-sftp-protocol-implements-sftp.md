---
layout: post
title: Golang SFTP 파일 전송 프로토콜 sftp 라이브러리 알아보기
---

오늘은 Golang으로 파일을 전송할 수 있는 SFTP 프로토콜을 구현한 sftp 라이브러리를 알아보려 합니다.

## sftp 설치

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

go get으로 sftp 패키지를 설치합니다.

```
go get github.com/pkg/sftp
```

홈의 go 폴더에 sftp 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/pkg/sftp"
	"golang.org/x/crypto/ssh"
)
```

fmt와 sftp 그리고 ssh를 가져옵니다.

```go
func main() {
	var con *ssh.Client
	client, err := sftp.NewClient(con)
	if err != nil {
		panic(err)
	}
```

ssh 클라이언트로 SFTP 클라이언트를 생성합니다.

```go
	defer client.Close()
```

defer 키워드로 작업이 종료될 때에 세션도 종료합니다.

```go
	walk := client.Walk("~/")
	for walk.Step() {
		if walk.Err() != nil {
			continue
		}
		fmt.Println(walk.Path())
	}
```

Walker로 지정한 경로에 위치하고, Step으로 다음 파일에 이동하면서 조회합니다.

```go
	file, err := client.Create("test.txt")
	if err != nil {
		panic(err)
	}
```

txt 파일을 만듭니다.

```go
	text := []byte("Hello world")
	if _, err := file.Write(text); err != nil {
		panic(err)
	}
```

만든 txt 파일에 바이트를 기록합니다.

```go
	out, err := sftp.Lstat("test.txt")
	if err != nil {
		panic(err)
	}
	fmt.Println(out)
}
```

txt 파일을 가져와서 FileInfo 형식으로 출력합니다.