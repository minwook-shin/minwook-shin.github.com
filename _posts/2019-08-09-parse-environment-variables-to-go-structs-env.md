---
layout: post
title: Golang 구조체로 환경 변수를 파싱하는 env 라이브러리 알아보기
---

오늘은 Golang의 구조체로 환경 변수를 파싱해서 사용할 수 있는 env 라이브러리를 알아보려 합니다.

## env 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 github에서 호스팅되고 있는 env 패키지를 설치합니다.

```
github.com/caarlos0/env
```

홈의 go 폴더에 env 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"time"

	"github.com/caarlos0/env"
)
```

fmt, time 그리고 env를 가져옵니다.

```go
type config struct {
	String   string        `env:"HOME"`
```

struct를 이용하여 환경 변수로 파싱할 수 있습니다.

HOME 환경 변수를 string 타입으로 가져올 수 있습니다.

```go
	Int      int           `env:"INT" envDefault:"1"`
```

int 타입도 가능하며, envDefault로 기본 값을 지정할 수 있습니다.

```go
	Bool     bool          `env:"PRODUCTION" envDefault:"true"`
```

bool 타입도 가능하며, envDefault로 기본 값을 지정할 수 있습니다.

```go
	Slice    []string      `env:"SLICE" envSeparator:":" envDefault:"hello:world"`
```

슬라이스로도 가능하며 envSeparator로 구분자를 지정할 수 있습니다.

```go
	Duration time.Duration `env:"DURATION"`
}
```

time.Duration로 지속 시간을 파싱할 수 있습니다.

```go
func main() {
	config := config{}
```

메인 함수에서 구조체로 config 객체를 만듭니다.

```go
	if err := env.Parse(&config); err != nil {
		panic(err)
	}
```

구조체를 파싱하고 그 값을 환경 변수에 불러옵니다.

```go
	fmt.Printf("%+v\n", config)
}
```

config 객체를 출력합니다.