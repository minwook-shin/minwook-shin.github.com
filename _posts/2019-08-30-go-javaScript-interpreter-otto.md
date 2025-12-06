---
layout: post
title: Golang 자바스크립트 인터프리터 otto 라이브러리 알아보기
---

오늘은 Golang으로 자바스크립트 구문을 파싱하고 인터프리터를 수행할 수 있는 otto 라이브러리를 알아보려 합니다.

## otto 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 otto 패키지를 설치합니다.

```
go get github.com/robertkrimen/otto
```

홈의 go 폴더에 otto 소스코드와 패키지 파일이 생성됩니다.


## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/robertkrimen/otto"
)
```

fmt와 otto를 가져옵니다.

```go
func main() {
	jsRuntime := otto.New()
	jsRuntime.Run(`test = 1 + 1;`)
```

자바스크립트 런타임을 만들고, 문자열로 이루어진 자바스크립트 코드를 수행합니다.

```go
	if value, err := jsRuntime.Get("test"); err == nil {
		if intValue, err := value.ToInteger(); err == nil {
			fmt.Println(intValue)
		}
	}
```

저장한 변수의 이름을 가지고 get을 호출하면 값을 otto.Value로 가져옵니다.

원하면 정수로 변환할 수 있습니다.

```go
	jsRuntime.Set("val", 10)
	jsRuntime.Run(`console.log(val);`)

	jsRuntime.Set("hello", "world")
	jsRuntime.Run(`console.log(hello);`)
```

해당 이름의 변수를 만들어서 값을 넣어주고, 자바스크립트 코드를 수행합니다.

```go
	value, err := jsRuntime.Run("hello.length")
	{
		returnValue, _ := value.ToInteger()
		fmt.Println(returnValue)
	}
```

자바스크립트의 length를 사용하여 "world" 문자열 길이를 측정할 수 있습니다.

```go
	jsRuntime.Set("jsValue", func(call otto.FunctionCall) otto.Value {
		return call.Argument(0)
	})

	result, _ := jsRuntime.Run(`jsValue(1.0);`)
	fmt.Println(result)
}
```

함수로 otto.Value를 반환하게 하고, 해당 함수를 자바스크립트 문자열에서 사용할 수 있습니다.