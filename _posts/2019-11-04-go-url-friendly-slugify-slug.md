---
layout: post
title: Golang 문자열 슬러그 생성 slug 라이브러리 알아보기
---

오늘은 Golang으로 유니코드 문자열에서 슬러그를 생성해주는 slug 라이브러리를 알아보려 합니다.

## slug 설치

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

go get으로 slug 패키지를 설치합니다.

```
go get -u github.com/gosimple/slug
```

홈의 go 폴더에 slug 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/gosimple/slug"
)
```

fmt와 slug를 가져옵니다.

```go
func main() {
	slugText := slug.Make("Hellö Wörld")
	fmt.Println(slugText)

	slugText = slug.Make("고")
	fmt.Println(slugText)
```

깔끔한 URL을 만들기 위해서 slugify 작업을 진행하여 slug를 생성합니다.

```go
	slugText = slug.MakeLang("python & go", "en")
	fmt.Println(slugText)
```

특정 언어에 맞게 slugify 작업을 진행하여 slug를 생성할 수 있습니다.

```go
	slug.Lowercase = false
	slug.MaxLength = 20
	slugText = slug.MakeLang("python & go", "en")
	fmt.Println(slugText)
```

소문자나 최대 길이를 지정하여 slug를 생성할 수 있습니다.

```go
	slug.CustomSub = map[string]string{
		"world": "hello",
	}
	slugText = slug.Make("hello world")
	fmt.Println(slugText)
```

커스텀 치환 map을 만들어서 slugify 작업을 진행하여 slug를 생성할 때에 문자를 바꿔줍니다.

```go
	isSlug := slug.IsSlug("hello-world")
	fmt.Println(isSlug)
}
```

공백, 문장 부호, 대소문자가 slugify 작업을 진행되었는 지 검사합니다.
