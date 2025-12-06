---
layout: post
title: Golang FTP 클라이언트 패키지 goftp 라이브러리 알아보기
---

오늘은 Golang으로 FTP 클라이언트 goftp 라이브러리를 알아보려 합니다.

## goftp 설치

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

go get으로 goftp 패키지를 설치합니다.

```
go get -u github.com/jlaffaye/ftp
```

홈의 go 폴더에 goftp 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"bytes"
	"fmt"
	"io/ioutil"
	"time"

	"github.com/jlaffaye/ftp"
)
```

bytes, fmt, io, time과 ftp를 가져옵니다.

```go
func main() {
	connect, err := ftp.Dial("127.0.0.1:21", ftp.DialWithTimeout(5*time.Second))
	if err != nil {
		panic(err)
	}
```

접속할 ftp 주소와 포트를 적고 타임아웃 시간까지 정하면 연결할 수 있습니다.

```go
	err = connect.Login("USER", "PASSWORD")
	if err != nil {
		panic(err)
	}
```

사용자 이름과 비밀번호로 서버에 인증합니다.

```go
	buffer := bytes.NewBufferString("Hello")
	err = connect.Stor("test.txt", buffer)
	if err != nil {
		panic(err)
	}
```

버퍼를 생성해서 서버에 텍스트 파일을 저장합니다.

```go
	response, err := connect.Retr("test.txt")
	if err != nil {
		panic(err)
	}

	byteData, err := ioutil.ReadAll(response)
	fmt.Println(string(byteData))
```

서버에 저장된 파일을 가져와서 바이트로 가져옵니다.

```go
	if err := connect.Quit(); err != nil {
		panic(err)
	}
}
```

마지막으로 서버의 연결을 종료합니다.