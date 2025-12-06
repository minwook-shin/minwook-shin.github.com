---
layout: post
title: Golang 웹 프레임워크 Gin 라이브러리 알아보기 2
---

오늘도 Golang으로 만들어진 martini과 유사한 웹 프레임워크 Gin 라이브러리를 알아보려 합니다.

## Gin 설치

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

go get으로 Gin 패키지를 설치합니다.

```
go get -u github.com/gin-gonic/gin
```

홈의 go 폴더에 Gin 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"io"
	"log"
	"net/http"
	"os"

	"github.com/gin-gonic/gin"
)
```

fmt, io, log, net, os 그리고 gin을 가져옵니다.

```go
func main() {
	engine := gin.Default()
```

Engine 객체를 생성합니다.

```go
	engine.POST("/post_query", func(c *gin.Context) {
		id := c.Query("id")
		page := c.DefaultQuery("Default", "0")
		c.String(http.StatusOK, id, page)
	})
```

쿼리와 함께 post 요청을 보내서 값을 반환받을 수 있습니다.

```go
	engine.POST("/post_map", func(c *gin.Context) {
		queryMap := c.QueryMap("map")
		postFormMap := c.PostFormMap("PostFormMap")

		fmt.Println(queryMap, postFormMap)
	})
```

쿼리와 함께 post 요청을 보내서 map을 반환받을 수 있습니다.

```go
	engine.POST("/file", func(c *gin.Context) {
		file, _ := c.FormFile("file")
		c.String(http.StatusOK, fmt.Sprintf("'%s'", file.Filename))
	})
```

파일을 반환하고 이름을 조회할 수 있습니다.

```go
	engine.GET("/json", func(c *gin.Context) {
		c.JSON(http.StatusOK, gin.H{"status": http.StatusOK})
	})
```

json 형식으로 값을 반환할 수 있습니다.

```go
	engine.GET("/redirect", func(c *gin.Context) {
		c.Redirect(http.StatusMovedPermanently, "http://www.google.com/")
	})
```

HTTP 리다이렉트로 인하여 해당 경로로 접근할 때 원하는 주소로 넘어갈 수 있습니다.

```go
	engine.GET("/test", func(c *gin.Context) {
		c.Request.URL.Path = "/test2"
		engine.HandleContext(c)
	})
	engine.GET("/test2", func(c *gin.Context) {
		c.JSON(200, gin.H{"hello": "world"})
	})
```

라우터 리다이렉트로 인하여 해당 경로로 접근할 때 원하는 주소의 값으로 넘어갈 수 있습니다.

```go
	gin.DebugPrintRouteFunc = func(httpMethod, absolutePath, handlerName string, nuHandlers int) {
		log.Printf("%v %v %v %v\n", httpMethod, absolutePath, handlerName, nuHandlers)
	}
```

디버그 로그 형식을 원하는 형식으로 지정할 수 있습니다.

```go
	engine.Run(":8080")
}
```

라우터를 http.Server에 연결하고 8080 포트에서 서버를 구동합니다.