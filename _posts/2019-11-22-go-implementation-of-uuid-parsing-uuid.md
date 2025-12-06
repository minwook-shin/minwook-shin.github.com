---
layout: post
title: Golang UUID 생성하고 파싱하는 UUID 라이브러리 알아보기
---

오늘은 Golang으로 Universally Unique Identifiers 생성하고 파싱하는 UUID 라이브러리를 알아보려 합니다.

## UUID 설치

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

go get으로 UUID 패키지를 설치합니다.

```
go get github.com/gofrs/uuid
```

홈의 go 폴더에 UUID 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/gofrs/uuid"
)
```

fmt와 uuid를 가져옵니다.

```go
func main() {
	uuid1, err := uuid.NewV4()
	if err != nil {
		panic(err)
	}
	fmt.Println(uuid1)
```

랜덤으로 UUID를 생성합니다.

```go
	gen := uuid.NewGen()
	uuid2, err := gen.NewV4()
	if err != nil {
		panic(err)
	}
	fmt.Println(uuid2)
```

Gen 객체를 만들어서 랜덤으로 UUID를 생성합니다.

```go
	var uuid3 = uuid.Must(uuid.NewV4())
	if err != nil {
		panic(err)
	}
	fmt.Println(uuid3)
```

오류를 반환할 수 있는 래핑 함수를 사용할 수 있습니다.

```go
	uuidStr := "1c880bea-a9af-41b7-ba89-f0b08ae3b741"
	uuid4, err := uuid.FromString(uuidStr)
	if err != nil {
		panic(err)
	}
	fmt.Println(uuid4)
}
```

문자열을 가져와서 파싱한 뒤에 UUID로 반환합니다.