---
layout: post
title: Golang 터미널 대화형 프롬프트 go-prompt 라이브러리 알아보기
---

오늘은 Golang으로 터미널에서 대화형 프롬프트를 만들 수 있는 go-prompt  라이브러리를 알아보려 합니다.

## go-prompt 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 github에서 호스팅되고 있는 go-prompt 패키지를 설치합니다.

```
go get github.com/c-bata/go-prompt
```

홈의 go 폴더에 go-prompt 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"os"
	"os/exec"

	prompt "github.com/c-bata/go-prompt"
)
```

fmt와 go-prompt를 가져오고, 추가로 bash를 실행하는 예제 코드를 위해 os도 추가로 가져옵니다.

```go
func executor(text string) {
	fmt.Println(text)
```

executor 함수를 구현하고 들어온 문자열을 그대로 출력하게 작성합니다.

```go
	if text == "bash" {
		cmd := exec.Command("bash")
		cmd.Stdin = os.Stdin
		cmd.Stdout = os.Stdout
		cmd.Stderr = os.Stderr
		cmd.Run()
	}
	return
}
```

만약 bash라고 입력되면 exec로 수행되어 실제 bash 환경이 구동됩니다.

```go
func completer(text prompt.Document) []prompt.Suggest {
	suggest := []prompt.Suggest{
		{Text: "english", Description: "english Description"},
		{Text: "한국어", Description: "한국어 설명"},

		{Text: "bash"},
	}
```

completer 함수를 구현하고 제안할 문장을 작성합니다.

```go
	return prompt.FilterHasPrefix(suggest, text.GetWordBeforeCursor(), true)
}
```

입력한 문장으로 커서의 앞에 존재하는 단어가 미리 설정한 제안하는 단어(prompt.Suggest)로 시작하는지 여부를 확인하는 함수를 반환합니다.

```go
func main() {
	mainPrompt := prompt.New(
		executor,
		completer,
		prompt.OptionTitle("prompt"),
		prompt.OptionPrefixTextColor(prompt.White),
		prompt.OptionPreviewSuggestionTextColor(prompt.Blue),
		prompt.OptionSelectedSuggestionBGColor(prompt.LightGray),
		prompt.OptionSuggestionBGColor(prompt.DarkGray),
	)
	mainPrompt.Run()
}
```

메인 함수에서는 미리 작성한 executor, completer 함수로 prompt 객체를 만드는 데에 사용합니다.

Option 접두사가 붙은 메소드를 생성하여 타이틀이나 색상 설정을 진행할 수 있습니다.
