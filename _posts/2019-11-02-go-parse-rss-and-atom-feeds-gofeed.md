---
layout: post
title: Golang RSS 및 atom 피드 파싱 gofeed 라이브러리 알아보기
---

오늘은 Golang으로 RSS와 atom 피드를 파싱하고 출력할 수 있는 gofeed 라이브러리를 알아보려 합니다.

## gofeed 설치

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

go get으로 gofeed 패키지를 설치합니다.

```
go get github.com/mmcdole/gofeed
```

홈의 go 폴더에 gofeed 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"os"

	"github.com/mmcdole/gofeed"
)
```

fmtl, os 그리고 gofeed를 가져옵니다.

```go
func main() {
	parser := gofeed.NewParser()
	feed, _ := parser.ParseURL("http://minwook-shin.github.io/feed")
	fmt.Println(feed)

	fmt.Println(feed.Title)
	fmt.Println(feed.Description)
	fmt.Println(feed.Items)
```

feed 파서를 만들어서 url로 feed를 가져올 수 있습니다.

```go
	feedData := `<rss version="2.0">
<channel>
<title>test</title>
</channel>
</rss>`
	parser = gofeed.NewParser()
	feed, _ = parser.ParseString(feedData)
	fmt.Println(feed.Title)
```

feed 파서를 만들어서 문자열로 feed를 가져올 수 있습니다.

```go
	file, _ := os.Open("test.xml")
	defer file.Close()
	parser = gofeed.NewParser()
	feed, _ = parser.Parse(file)
	fmt.Println(feed)
}
```

feed 파서를 만들어서 파일로 feed를 가져올 수 있습니다.