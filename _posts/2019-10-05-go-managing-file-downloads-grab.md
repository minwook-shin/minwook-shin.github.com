---
layout: post
title: Golang 파일 다운로드 grab 라이브러리 알아보기
---

오늘은 Golang으로 파일을 내려받을 때에 관리할 수 있는 grab 라이브러리를 알아보려 합니다.

## grab 설치

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

go get으로 grab 패키지를 설치합니다.

```
go get github.com/cavaliercoder/grab
```

홈의 go 폴더에 grab 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"time"

	"github.com/cavaliercoder/grab"
)
```

fmt, time 그리고 grab을 가져옵니다.

```go
func main() {
	client := grab.NewClient()
	request, _ := grab.NewRequest(".", "http://www.oreilly.com/programming/free/files/functional-programming-python.pdf")
```

파일 다운로드 클라이언트를 생성하고 파일을 요청합니다.

```go
	fmt.Println(request.URL())
```

다운로드하는 URL을 반환하여 출력합니다.

```go
	response := client.Do(request)
	fmt.Println(response.HTTPResponse.Status)
```

파일 전송을 할 때 돌아오는 응답을 처리합니다.

Status도 확인할 수 있습니다.

```go
	ticker := time.NewTicker(100 * time.Millisecond)
	defer ticker.Stop()
```

지정된 시간으로 채널이 포함된 ticker를 반환합니다.

```go
Loop:
	for {
		select {
		case <-ticker.C:
			fmt.Printf("%v / %v (%.2f%%)\n",
				response.BytesComplete(),
				response.Size(),
				100*response.Progress())

		case <-response.Done:
			break Loop
		}
	}
```

바이트를 복사하며 반복하다가 다운로드가 완료되면 Done으로 빠져 루프를 멈춥니다.

```go
	err := response.Err()
	if err != nil {
		panic(err)
	}
```

만약 오류가 발생하면 panic을 일으킵니다.

```go
	fmt.Println("Saved : ", response.Filename)
}
```

파일 이름을 반환하여 출력합니다.