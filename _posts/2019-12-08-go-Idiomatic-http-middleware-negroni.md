---
layout: post
title: Golang HTTP 미들웨어 Negroni 라이브러리 알아보기
---

오늘은 Golang으로 기본 net/http 패키지와 같이 사용할 수 있는 HTTP 미들웨어 Negroni 라이브러리를 알아보려 합니다.

## Negroni 설치

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

go get으로 Negroni 패키지를 설치합니다.

```
go get github.com/urfave/negroni
```

홈의 go 폴더에 Negroni 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"net/http"

	"github.com/urfave/negroni"
)
```

fmt, net 그리고 negroni를 가져옵니다.

```go
func main() {
	serveMux := http.NewServeMux()
```

ServeMux를 생성합니다.

```go
	serveMux.HandleFunc("/", func(w http.ResponseWriter, req *http.Request) {
		fmt.Fprintf(w, "hello, world!")
	})
```

특정 경로에 작동할 핸들러 함수를 등록합니다.

```go
	negroniObj := negroni.Classic()
```

복구, 로그와 같은 기본 미들웨어가 내장된 객체를 생성합니다.

```go
	negroniObj.UseHandler(serveMux)
```

만들어둔 ServeMux를 사용합니다.

```go
	http.ListenAndServe(":8080", negroniObj)
}
```

8080 포트로 서버를 구동합니다.