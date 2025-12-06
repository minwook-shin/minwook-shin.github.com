---
layout: post
title: Golang 터미널 문자 색상 바꾸는 Aurora 라이브러리 알아보기
---

오늘은 Golang으로 터미널에서 fmt 출력할 때에 문자열 색상을 지정할 수 있는 Aurora 라이브러리를 알아보려 합니다.

## Aurora 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 github에서 호스팅되고 있는 Aurora 패키지를 설치합니다.

```
go get -u github.com/logrusorgru/aurora
```

홈의 go 폴더에 Aurora 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	. "github.com/logrusorgru/aurora"
)
```

fmt와 aurora를 가져옵니다.

```go
func main() {
	fmt.Println(Bold("Bold"))
	fmt.Println(Faint("Faint"))
	fmt.Println(DoublyUnderline("DoublyUnderline"))
	fmt.Println(Fraktur("Fraktur"))
	fmt.Println(Italic("Italic"))
	fmt.Println(Underline("Underline"))
	fmt.Println(SlowBlink("SlowBlink"))
	fmt.Println(RapidBlink("RapidBlink"))
	fmt.Println(Blink("Blink"))
	fmt.Println(Reverse("Reverse"))
	fmt.Println(Inverse("Inverse"))
	fmt.Println(Conceal("Conceal"))
	fmt.Println(Hidden("Hidden"))
	fmt.Println(CrossedOut("CrossedOut"))
	fmt.Println(StrikeThrough("StrikeThrough"))
	fmt.Println(Framed("Framed"))
	fmt.Println(Encircled("Encircled"))
	fmt.Println(Overlined("Overlined"))
```

다양한 포맷을 터미널에서 표현할 수 있습니다.

```go
	fmt.Println(Black("Black"))
	fmt.Println(Red("Red"))
	fmt.Println(Green("Green"))
	fmt.Println(Yellow("Yellow"))
	fmt.Println(Brown("Brown"))
	fmt.Println(Blue("Blue"))
	fmt.Println(Magenta("Magenta"))
	fmt.Println(Cyan("Cyan"))
	fmt.Println(White("White"))
	fmt.Println(BrightBlack("BrightBlack"))
	fmt.Println(BrightRed("BrightRed"))
	fmt.Println(BrightGreen("BrightGreen"))
	fmt.Println(BrightYellow("BrightYellow"))
	fmt.Println(BrightBlue("BrightBlue"))
	fmt.Println(BrightMagenta("BrightMagenta"))
	fmt.Println(BrightCyan("BrightCyan"))
	fmt.Println(BrightWhite("BrightWhite"))
```

다양한 색상을 터미널에서 표현할 수 있습니다.

```go
	fmt.Println(BgBlack("BgBlack"))
	fmt.Println(BgRed("BgRed"))
	fmt.Println(BgGreen("BgGreen"))
	fmt.Println(BgYellow("BgYellow"))
	fmt.Println(BgBrown("BgBrown"))
	fmt.Println(BgBlue("BgBlue"))
	fmt.Println(BgMagenta("BgMagenta"))
	fmt.Println(BgCyan("BgCyan"))
	fmt.Println(BgWhite("BgWhite"))
	fmt.Println(BgBrightBlack("BgBrightBlack"))
	fmt.Println(BgBrightRed("BgBrightRed"))
	fmt.Println(BgBrightGreen("BgBrightGreen"))
	fmt.Println(BgBrightYellow("BgBrightYellow"))
	fmt.Println(BgBrightBlue("BgBrightBlue"))
	fmt.Println(BgBrightMagenta("BgBrightMagenta"))
	fmt.Println(BgBrightCyan("BgBrightCyan"))
	fmt.Println(BgBrightWhite("BgBrightWhite"))
```

다양한 문자열 배경을 터미널에서 표현할 수 있습니다.

```go
	hello := Blue("hello world").Bold().BgBrightWhite()
	fmt.Println(hello)
```

위 색상과 포맷을 체인 형식으로 작성할 수도 있습니다.

```go
	fmt.Println(
		Gray(0, " 1 ").BgGray(23),
		Gray(3, " 2 ").BgGray(19),
		Gray(7, " 3 ").BgGray(15),
		Gray(11, " 4 ").BgGray(11),
		Gray(15, " 5 ").BgGray(9),
		Gray(19, " 6 ").BgGray(3),
		Gray(23, " 7 ").BgGray(0),
	)
}
```

Grayscale도 표현할 수 있습니다.