---
layout: post
title: Golang 웹 프레임워크 Gin 라이브러리 알아보기 1
---

오늘은 Golang으로 만들어진 martini과 유사한 웹 프레임워크 Gin 라이브러리를 알아보려 합니다.

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
	engine.GET("/default", func(c *gin.Context) {
		c.JSON(200, gin.H{
			"message": "hello",
		})
	})
```

get 요청을 보내서 값을 반환받을 수 있습니다.

```go
	engine.Use(gin.Logger())

	engine.Use(gin.Recovery())
```

전역 설정으로 로거와 패닉을 복구하는 미들웨어를 사용할 수 있습니다.

```go
	file, _ := os.Create("gin.log")
	gin.DefaultWriter = io.MultiWriter(file)
```

로그를 파일로 남길 수 있습니다.

```go
	engine.GET("/param/:test", func(c *gin.Context) {
		val := c.Param("test")
		c.String(http.StatusOK, val)
	})
	engine.GET("/param/:test/*action", func(c *gin.Context) {
		val := c.Param("test")
		action := c.Param("action")
		message := val + " " + action

		fmt.Println(c.FullPath() == "/param/:test/*action")

		c.String(http.StatusOK, message)
	})
```

경로에 파라미터로 가져올 수 있습니다.

```go
	engine.GET("/query", func(c *gin.Context) {
		DefaultQuery := c.DefaultQuery("DefaultQuery", "true")
		query := c.Query("q")

		c.String(http.StatusOK, DefaultQuery, query)
	})
```

쿼리 스트링으로 값을 받아오거나 기본 값을 지정할 수 있습니다.

```go
	engine.POST("/post", func(c *gin.Context) {
		postForm := c.PostForm("PostForm")
		defaultPostForm := c.DefaultPostForm("DefaultPostForm", "true")

		c.JSON(200, gin.H{
			"PostForm":        postForm,
			"DefaultPostForm": defaultPostForm,
		})
	})
```

post 요청을 보내서 값을 반환받을 수 있습니다.

```go
	engine.Run(":8080")
}
```

라우터를 http.Server에 연결하고 8080 포트에서 서버를 구동합니다.

> 내일 다음 포스트가 업로드됩니다.