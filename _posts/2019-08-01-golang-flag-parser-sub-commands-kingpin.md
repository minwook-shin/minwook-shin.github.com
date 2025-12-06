---
layout: post
title: Golang CLI 인자 파싱하는 Kingpin 라이브러리 알아보기
---

오늘은 Golang에서 플루언트 인터페이스처럼 CLI 인자 파싱하는 Kingpin 라이브러리를 알아보려 합니다.

## Kingpin 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 github에서 호스팅되고 있는 Kingpin 패키지를 설치합니다.

```
go get gopkg.in/alecthomas/kingpin.v2
```

홈의 go 폴더에 Kingpin 소스코드와 패키지 파일이 생성됩니다.

## 예제

간단하게 플래그와 인자를 사용할 수 있는 예제를 만들어보려 합니다.

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"gopkg.in/alecthomas/kingpin.v2"
)
```

kingpind을 가져옵니다.

```go
var (
	str = kingpin.Flag("str", "string description").String()
```

문자열을 플래그로 받을 수 있습니다.

```go
	bool = kingpin.Flag("bool", "bool description").Bool()
```

논리 타입도 플래드로 받을 수 있습니다.

```go
	duration = kingpin.Flag("duration", "duration description").Required().Short('t').Duration()
```

반드시 넣여야 될 정보의 인자면 Required를 부여할 수 있습니다.

```go
	ip = kingpin.Arg("ip", "IP description").Required().IP()
```

IP 형식의 문자열도 인자로 받을 수 있습니다.

```go
	url = kingpin.Arg("url", "url description").Required().URL()
```

URL 형식의 문자열도 인자로 받을 수 있습니다.

```go
	number = kingpin.Arg("int", "int description").Int()
)
```

위 문자열과 마찬가지로 정수도 인자로 받을 수 있습니다.

```go
func main() {
	kingpin.Version("0.0.1")
	kingpin.Parse()
}
```

이제 메인 함수에서 인자를 파싱하고, 작성한 go 파일을 실행합니다.