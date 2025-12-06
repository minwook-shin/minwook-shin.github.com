---
layout: post
title: Golang jQuery와 유사한 구문 및 기능 goquery 라이브러리 알아보기
---

오늘은 Golang으로 jQuery와 유사한 구문과 기능을 가진 goquery 라이브러리를 알아보려 합니다.

## goquery 설치

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

go get으로 goquery 패키지를 설치합니다.

```
go get github.com/PuerkitoBio/goquery
```

홈의 go 폴더에 goquery 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"net/http"

	"github.com/PuerkitoBio/goquery"
)
```

fmt, http 그리고 goquery를 가져옵니다.

```go
func main() {
	response, err := http.Get("https://minwook-shin.github.io")
	if err != nil {
		panic(err)
	}
```

원하는 페이지를 가져옵니다.

```go
	defer response.Body.Close()
```

defer 키워드로 response.Body의 Close()를 호출합니다.

```go
	if response.StatusCode != 200 {
		panic(response.StatusCode)
	}
```

만약 정상적으로 페이지를 받아오지 못하면 panic을 일으킵니다.

```go
	document, err := goquery.NewDocumentFromReader(response.Body)
	if err != nil {
		panic(err)
	}
```

Document를 반환합니다.

```go
	document.Find(".posts article .entry").Each(func(i int, s *goquery.Selection) {
		titleText := s.Find("p").Text()
		fmt.Println(titleText)
	})
}
```

반환한 Document에서 selector로 값을 필터링하여 출력합니다.