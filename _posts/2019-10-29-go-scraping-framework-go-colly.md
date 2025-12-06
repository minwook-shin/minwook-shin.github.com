---
layout: post
title: Golang Scraping 프레임워크 Colly 라이브러리 알아보기
---

오늘은 Golang으로 만들어진 웹 사이트 Scraping 프레임워크 Colly 라이브러리를 알아보려 합니다.

## Colly 설치

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

go get으로 Colly 패키지를 설치합니다.

```
go get -u github.com/gocolly/colly
```

홈의 go 폴더에 Colly 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/gocolly/colly"
)
```

fmt와 colly를 가져옵니다.

```go
func main() {
collector := colly.NewCollector(
		colly.AllowedDomains("minwook-shin.github.io"),
		colly.MaxDepth(1),
		colly.Async(true),
	)
```

도메인 화이트리스트, 재귀 깊이, 비동기에 대한 설정을 포함한 Collector 객체를 만듭니다.

```go
	collector.OnHTML("a[href]", func(e *colly.HTMLElement) {
		link := e.Attr("href")
		fmt.Println(e.Text, link)
```

OnHTML을 등록하여 a 태그의 href 속성을 반환합니다.

그리고 텍스트와 링크를 출력합니다.

```go
		collector.Visit(e.Request.AbsoluteURL(link))
	})
```

계속해서 a 태그의 href 속성에 존재하는 링크를 Scraping 합니다.

```go
	collector.OnRequest(func(r *colly.Request) {
		fmt.Println(r.URL.String())
	})
```

OnRequest를 등록하여 HTTP 요청을 보낼 때마다 URL을 출력하게 합니다.

```go
	collector.Visit("https://minwook-shin.github.io")
}
```

수집하고자 하는 웹 사이트를 Scraping 합니다.