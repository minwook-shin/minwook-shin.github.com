---
layout: post
title: Golang URL 텍스트 추출 xurls 라이브러리 알아보기
---

오늘은 Golang으로 텍스트에서 URL들을 추출하는 xurls 라이브러리를 알아보려 합니다.

## xurls 설치

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

go get으로 xurls 패키지를 설치합니다.

```
go get github.com/mvdan/xurls
```

홈의 go 폴더에 xurls 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/mvdan/xurls"
)
```

fmt 그리고 xurls를 가져옵니다.

```go
func main() {
	rxRelaxed := xurls.Relaxed()
	relaxed := rxRelaxed.FindString("hello, world! test.org!")
	fmt.Println(relaxed)
```

스키마가 없는 URL 형식도 포함한 모든 URL들을 찾아서 추출합니다.

```go
	relaxed = rxRelaxed.FindString("This is not URL")
	fmt.Println(relaxed)
```

URL들을 찾아서 없으면 빈 값이 반환됩니다.

```go
	rxStrict := xurls.Strict()
	strict := rxStrict.FindAllString("See http://example.com/.", -1)
	fmt.Println(strict)
```

스키마가 갖추어진 URL 형식을 추출하여 리스트로 반환합니다.

```go
	strict = rxStrict.FindAllString("hello, world! test.org!", -1)
	fmt.Println(strict)
```

기존 Relaxed에서 추출된 URL이라도 스키마가 없으면 추출되지 않습니다.

```go
	regexp, _ := xurls.StrictMatchingScheme(`^[a-z]+`)
	fmt.Println(regexp.MatchString("http://hello.com"))
}
```

원하는 정규식을 따로 지정할 수도 있습니다.