---
layout: post
title: Golang 터미널 출력을 업데이트하는 uilive 라이브러리 알아보기
---

오늘은 Golang으로 터미널 출력을 실시간으로 업데이트하여 다운로드, 업데이트 화면을 구현할 때에 사용되는 uilive 라이브러리를 알아보려 합니다.

## uilive 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 github에서 호스팅되고 있는 uilive 패키지를 설치합니다.

```
go get -v github.com/gosuri/uilive
```

홈의 go 폴더에 uilive 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"time"

	"github.com/gosuri/uilive"
)
```

fmt와 time, 그리고 uilive를 가져옵니다.

```go
func main() {
	writer := uilive.New()
	writer.Start()
```

새로운 writer 객체를 생성하고 터미널에 업데이트하는 리스너를 시작합니다.

```go
	for i := 0; i <= 100; i++ {
		fmt.Fprintf(writer, "Downloading.. %d\n", i)
		fmt.Fprintf(writer.Newline(), "Downloading.. %d\n", i)
```

for 문을 이용하여 100까지 반복하여 writer 객체에 텍스트를 기록합니다.

다만, 터미널 출력을 실시간 업데이트하므로 io.Writer로 새로운 라인을 생성한 것을 제외하면 한 줄로 처리됩니다.

```go
		time.Sleep(time.Millisecond * 5)
	}
```

for문이 빨리 지나가므로 잠시 멈추어줍니다.

```go
	fmt.Fprintln(writer, "Finished")

	fmt.Fprintf(writer.Bypass(), "Downloaded files\n")
```

writer.Bypass()에 의해 non-buffered 출력이 먼저 터미널에 나타나게 됩니다.

```go
	writer.Stop()
}
```

마지막으로 터미널에 업데이트하는 리스너를 멈춥니다.