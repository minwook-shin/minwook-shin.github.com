---
layout: post
title: Golang 웹 프레임워크 Echo 라이브러리 알아보기
---

오늘은 Golang으로 확장할 수 있는 미들웨어 웹 프레임워크 Echo 라이브러리를 알아보려 합니다.

## Echo 설치

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

go get으로 Echo 패키지를 설치합니다.

```
go get github.com/labstack/echo
```

홈의 go 폴더에 Echo 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"github.com/labstack/echo"
	"github.com/labstack/echo/middleware"
	"net/http"
)
```

net과 echo를 가져옵니다.

```go
func main() {
	e := echo.New()
```

echo 객체를 만듭니다.

```go
	e.Use(middleware.Logger())
	e.Use(middleware.Recover())
```

라우터가 수행된 다음에 미들웨어를 사용할 수 있게 합니다.

HTTP 요청을 기록하고 패닉 복구도 할 수 있는 미들웨어를 사용합니다.

```go
	e.GET("/", handler)
```

만든 핸들러 함수로 GET 방식으로 호출합니다.

```go
	err := e.Start(":8000")
	if err != nil {
		e.Logger.Fatal(err)
	}
}
```

HTTP 서버를 구동합니다.

```go
func handler(c echo.Context) error {
	return c.String(http.StatusOK, "Hello, World!")
}
```

핸들러 함수를 구현합니다.