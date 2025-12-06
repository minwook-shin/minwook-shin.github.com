---
layout: post
title: Golang 마이크로 웹 프레임워크 Tango 라이브러리 알아보기
---

오늘은 Golang으로 마이크로, 미들웨어를 장착할 수 있는 웹 프레임워크 Tango 라이브러리를 알아보려 합니다.

## Tango 설치

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

go get으로 Tango 패키지를 설치합니다.

```
go get github.com/lunny/tango
```

홈의 go 폴더에 Tango 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"github.com/lunny/tango"
)
```

tango를 가져옵니다.

```go
type jsonAction struct {
	tango.JSON
}
```

JSON으로 반환될 수 있게 구조체를 생성합니다.

```go
func (jsonAction) Get() interface{} {
	return map[string]string{
		"message": "Hello world!",
	}
}
```

Get 메소드를 구현합니다.

페이지가 열릴 때에 {"message":"Hello world!"}로 출력됩니다.

```go
func main() {
	tango := tango.Classic()
```

기본 핸들러와 로거를 가진 객체를 생성합니다.

```go
	tango.Get("/", new(jsonAction))
```

라우터를 생성합니다.

```go
	tango.Server.Addr = "0.0.0.0:8000"
	tango.Server.Handler = tango
```

서버 주소와 헨들러를 지정합니다.

```go
	err := tango.ListenAndServe()
	if err != nil {
		panic(err)
	}
}
```

미리 작성한 서버 주소와 헨들러를 기반으로 구동합니다.