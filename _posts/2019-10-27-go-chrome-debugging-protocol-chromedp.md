---
layout: post
title: Golang 크롬 디버깅 프로토콜 chromedp 라이브러리 알아보기
---

오늘은 Golang으로 크롬 디버깅 프로토콜로 웹 페이지를 테스트하는 chromedp 라이브러리를 알아보려 합니다.

## chromedp 설치

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

go get으로 chromedp 패키지를 설치합니다.

```
go get -u github.com/chromedp/chromedp
```

홈의 go 폴더에 chromedp 소스코드와 패키지 파일이 생성됩니다.

만약 구글 크롬이 설치되어 있지 않으며, 아래와 같이 오류가 출력됩니다.

"google-chrome": executable file not found in $PATH

해당 오류가 발생하면 구글 크롬을 설치하고 재시도합니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"context"
	"fmt"
	"log"
	"time"

	"github.com/chromedp/chromedp"
)
```

context, fmt, log, time 그리고 chromedp를 가져옵니다.

```go
func main() {
	contextVar, cancelFunc := chromedp.NewContext(
		context.Background(),
		chromedp.WithLogf(log.Printf),
	)
	defer cancelFunc()
```

chromedp 컨텍스트를 만듭니다.

```go
	contextVar, cancelFunc = context.WithTimeout(contextVar, 10*time.Second)
	defer cancelFunc()
```

만든 chromedp 컨텍스트로 타임아웃을 설정합니다.

```go
	var strVar string
```

가져온 데이터를 저장할 string 변수를 만듭니다.

```go
	err := chromedp.Run(contextVar,
		chromedp.Navigate(`https://golang.org/pkg/fmt/`),
		chromedp.WaitVisible(`body > footer`),
		chromedp.Click(`#pkg-examples > div`, chromedp.NodeVisible),
		chromedp.Value(`#example_Println .play .input textarea`, &strVar),
	)
	if err != nil {
		panic(err)
	}
```

https://golang.org/pkg/fmt 사이트 주소로 접근하여 footer 태그 부분이 랜더링될 때까지 기다리고, 원하는 버튼을 클릭하여 텍스트를 가져옵니다.

```go
	fmt.Println(strVar)
}
```

원하는 텍스트를 저장한 strVar 변수를 출력합니다.