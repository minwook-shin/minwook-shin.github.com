---
layout: post
title: Golang 코드 생성하는 Jennifer 라이브러리 알아보기
---

오늘은 Golang으로 템플릿없이 임의의 Go 언어 코드를 생성할 수 있는 Jennifer 라이브러리를 알아보려 합니다.

## Jennifer 설치

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

go get으로 Jennifer 패키지를 설치합니다.

```
go get -u github.com/dave/jennifer/jen
```

홈의 go 폴더에 Jennifer 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/dave/jennifer/jen"
)
```

fmt와 jen을 가져옵니다.

```go
func main() {
	f := jen.NewFile("main")
```

인자로 패키지 이름을 받아서 새로운 이름의 파일을 만듭니다.

```go
	f.Func().Id("main").Params().Block(
```

함수 키워드를 가지고, main 함수를 만듭니다.

이어서 파라미터와 블럭을 작성합니다.

```go
		jen.Qual("fmt", "Println").Call(jen.Lit("Hello, world")),
	)
```

fmt.Println 를 생성하고, 고정된 리터럴 값을 부여합니다.

```go
	fmt.Printf("%#v", f)
}
```

%#v 포맷과 Printf로 Go언어의 구문 표현하는 소스 코드 스니펫을 출력합니다.

```go
package main

import "fmt"

func main() {
        fmt.Println("Hello, world")
}
```

지금까지 실습을 진행하면 터미널에 위와 같이 출력됩니다.