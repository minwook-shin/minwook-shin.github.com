---
layout: post
title: Golang deep pretty printer Go-spew 라이브러리 알아보기
---

오늘은 Golang으로 자료 구조에 도움이 되는 deep pretty printer로 출력해주는 Go-spew 라이브러리를 알아보려 합니다.

## Go-spew 설치

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

go get으로 Go-spew 패키지를 설치합니다.

```
go get -u github.com/davecgh/go-spew/spew
```

홈의 go 폴더에 Go-spew 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"net/http"

	"github.com/davecgh/go-spew/spew"
)
```

fmt, http 그리고 spew를 가져옵니다.

```go
func test() {
	testVal := 1
	spew.Dump(testVal)

	type testStruct struct {
		ID   int64  `json:"project_id"`
		Name string `json:"name"`
	}

	t := testStruct{ID: 1, Name: "TEST"}
	spew.Dump(t)
}
```

int64 변수와 구조체를 만들고 spew로 덤프합니다.

```go
func handler(w http.ResponseWriter, r *http.Request) {
	w.Header().Set("Content-Type", "text/html")
	fmt.Fprintf(w, "Hello World!")
	fmt.Println(spew.Sdump(w))
}
```

서버 핸들러로 추가해서 웹 서버에서 디버깅을 할 때도 사용할 수 있습니다.

```go
func main() {
	test()
```

처음 int64 변수와 구조체에 덤프를 하면 아래와 같이 출력됩니다.

(int) 1
(main.testStruct) {
 ID: (int64) 1,
 Name: (string) (len=4) "TEST"
}

```go
	http.HandleFunc("/", handler)
	http.ListenAndServe(":8080", nil)
}
```

웹 서버를 구동하고 최초 접속을 해야 핸들러 함수가 동작하여 덤프한 결과물을 볼 수 있습니다.