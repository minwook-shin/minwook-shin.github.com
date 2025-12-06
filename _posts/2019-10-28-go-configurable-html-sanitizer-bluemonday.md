---
layout: post
title: Golang HTML sanitizer bluemonday 라이브러리 알아보기
---

오늘은 Golang으로 웹 페이지의 컨텐츠를 안전하게 가져올 수 있는 HTML sanitizer bluemonday 라이브러리를 알아보려 합니다.

## bluemonday 설치

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

go get으로 bluemonday 패키지를 설치합니다.

```
go get -u github.com/microcosm-cc/bluemonday
```

홈의 go 폴더에 bluemonday 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/microcosm-cc/bluemonday"
)
```

fmt와 bluemonday를 가져옵니다.

```go
func main() {
	policy := bluemonday.UGCPolicy()
```

HTML WYSIWYG와 마크다운 문법으로 변환하는 정책을 반환합니다.

```go
	htmlString:= policy.Sanitize(
		`<a onclick="alert("hello")" href="http://www.google.com">Google</a>`,
	)

	fmt.Println(htmlString)
```

HTML 문자열을 가져와서 UGCPolicy를 적용한 문자열로 반환합니다.

```go
	policy = bluemonday.NewPolicy()
```

아무것도 포함되지 않는 정책을 반환합니다.

```go
	policy.AllowStandardURLs()
```

rel="nofollow" 와 같은 요소를 포함할 수 있습니다.

```go
	policy.AllowAttrs("href").OnElements("a")
	policy.AllowElements("p")
```

허용할 HTML attribute와 elements를 지정할 수 있습니다.

```go
	htmlString = policy.Sanitize(
		`<p><a onclick="alert("hello")" href="http://www.google.com">Google</a><p/>`,
	)
	fmt.Println(htmlString)
}
```

HTML 문자열을 가져와서 커스터마이징한 정책이 적용된 문자열로 반환합니다.
