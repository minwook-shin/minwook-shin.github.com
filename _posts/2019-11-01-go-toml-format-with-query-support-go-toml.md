---
layout: post
title: Golang TOML 포맷 go-toml 라이브러리 알아보기
---

오늘은 Golang으로 TOML 포맷 Marshal, Unmarshal을 할 수 있는 go-toml 라이브러리를 알아보려 합니다.

## go-humanize 설치

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

go get으로 go-toml 패키지를 설치합니다.

```
go get github.com/pelletier/go-toml
```

홈의 go 폴더에 go-toml 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/pelletier/go-toml"
)
```

fmt와 go-toml을 가져옵니다.

```go
func main() {
	type TestData struct {
		Default string `toml:"default"`
		Comment string `toml:"comment" commented:"true" comment:"hello world"`
		Data    string `toml:"data" commented:"false" comment:"hello world"`
	}
	type TestSection struct {
		TestData `toml:"Section" comment:"Test Section"`
	}
```

구조체를 만듭니다.

이 때에 commented, comment로 해당 필드를 주석 처리하거나, 주석을 달 수 있습니다.

```go
	section := TestSection{TestData{Default: "default_id", Comment: "test comment", Data: "test data"}}

	byteData, err := toml.Marshal(section)
	if err != nil {
		panic(err)
	}
```

만든 객체로 Marshal을 하면 바이트 코드로 반환됩니다.

```go
	fmt.Println(string(byteData))
```

해당 반환을 출력해보면 toml 형식으로 출력합니다.

```go
	section = TestSection{}
	toml.Unmarshal(byteData, &section)
	fmt.Println(section)
}
```

toml 형식의 바이트를 다시 객체로 Unmarshal합니다.