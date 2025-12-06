---
layout: post
title: Golang 에러 핸들러 errors 라이브러리 알아보기
---

오늘은 Golang으로 디버그를 할 때 유용하도록 오류가 발생하는 경로에 컨텍스트를 부여해주는 핸들러인 errors 라이브러리를 알아보려 합니다.

## errors 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 errors 패키지를 설치합니다.

```
go get github.com/pkg/errors
```

홈의 go 폴더에 errors 소스코드와 패키지 파일이 생성됩니다.


## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/pkg/errors"
)
```

fmt와 errors를 가져옵니다.

```go
func main() {
	err := errors.New("ERROR")
	fmt.Printf("%+v", err)
```

인자로 준 문자열과 함께 오류를 발생시킵니다.

%+v 포맷으로 인하여 오류의 경로를 따라갈 수 있는 내용들이 출력됩니다.

```go
	cause := errors.New("ERROR")
	err = errors.WithMessage(cause, "new message.")
	fmt.Printf("%+v", err)
```

WithMessage에 의하여 에러 메시지를 달 수 있습니다.

```go
	cause = errors.New("ERROR")
	err = errors.WithStack(cause)
	fmt.Printf("%+v", err)
```

WithStack에 의하여 스택 추적을 할 때에 에러 메시지를 남길 수 있습니다.

```go
	cause = errors.New("ERROR")
	err = errors.Wrap(cause, "wrap")
	fmt.Printf("%+v", err)
```

Wrap으로 에러 메시지를 출력하면서 에러를 출력할 수 있습니다.

```go
	cause = errors.New("ERROR")
	err = errors.Wrapf(cause, "wrap : %d", 1)
	fmt.Printf("%+v", err)
```

기존 Wrap에서 포맷 스트링을 사용할 수 있습니다.

```go
	err = errors.Errorf("ERROR : %s", "test")
	fmt.Printf("%+v", err)
```

지정항 포맷 스트링에 맞게 대체된 다음에 에러 메시지로 출력될 수 있습니다.

```go
	err = errors.Wrap(func() error {
		return func() error {
			return errors.Errorf("ERROR : %s", fmt.Sprintf("test"))
		}()
	}(), "wrap")

	fmt.Printf("%+v", err)
}
```

함수의 반환 값인 함수에서 에러가 발생하고, 이어서 wrap이라는 에러 메시지도 출력됩니다.