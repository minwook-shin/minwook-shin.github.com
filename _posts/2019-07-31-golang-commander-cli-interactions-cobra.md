---
layout: post
title: Golang CLI 응용 프로그램을 만드는 Cobra 라이브러리 알아보기
---

오늘은 Golang에서 CLI 응용 프로그램을 만드는 Cobra 라이브러리를 알아보려 합니다.

## Cobra 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 github에서 호스팅되고 있는 cobra 패키지를 설치합니다.

```
go get -u github.com/spf13/cobra/cobra
```

홈의 go 폴더에 cobra 소스코드와 패키지 파일이 생성됩니다.

## 예제

간단하게 플래그와 커맨드를 사용할 수 있는 예제를 만들어보려 합니다.

```
- main.go
	/cmd
		- root.og
		- version.go
```

위와 같이 폴더 구조가 되어 있으면 main.go 파일로 flags와 command를 사용할 수 있게 됩니다.

## main

```go
package main

import (
	"./cmd"
)

func main() {
	cmd.Execute()
}
```

우선, 메인 파일에서 root의 Execute를 실행되도록 합니다.

## root

```go
package cmd
```

cmd라는 패키지로 만들어서 main 함수에서 불러올 수 있게 합니다.

```go
import (
	"fmt"
	"os"

	"github.com/spf13/cobra"
)
```

fmt와 os, 그리고 cobra를 가져옵니다.

```go
func init() {
	StringVar := ""
	StringVarP := ""
	rootCmd.PersistentFlags().StringVar(&StringVar, "stringVar", "", "StringVar defines a string flag")
	rootCmd.PersistentFlags().StringVarP(&StringVarP, "stringVarP", "v", "", "like StringVar, but accepts a shorthand letter")
	rootCmd.PersistentFlags().String("String", "", "String defines a string flag")
	rootCmd.PersistentFlags().StringP("stringP", "p", "", "like String, but accepts a shorthand letter")
}
```

생성자로 StringVar, StringVarP, String 그리고 StringP에 대한 플래그를 만들어 보았습니다.

문자열외에도 숫자, 논리 타입에 대해서도 플래그를 설정할 수도 있습니다. 

* StringVar : 플래그 이름, 기본 값, 사용법으로 작성되며, 값을 문자열 변수로 저장할 수 있습니다.

* StringVarP : StringVar와 유사하지만, 단축문자를 설정할 수 있습니다.

* String : 플래그 이름, 기본 값, 사용법으로 작성할 수 있습니다.

* StringP : String과 유사하지만, 단축문자를 설정할 수 있습니다.

```go
var rootCmd = &cobra.Command{
	Use:   "main.go",
	Short: "A 'Hello, World!' program generally is a computer program that outputs or displays the message 'Hello, World!'.",
	Long: `A "Hello, World!" program is 
traditionally used to introduce 
novice programmers to a programming language.`,
	Run: func(cmd *cobra.Command, args []string) {
		fmt.Print("hello world!")
	},
}
```

cobra의 Command를 통해 커맨드를 만듭니다.

기본적으로 Run에 의해서 hello world 문자열이 출력되며, help 커맨드를 통해 설명이 출력됩니다.

```go
func Execute() {
	if err := rootCmd.Execute(); err != nil {
		fmt.Println(err)
		os.Exit(1)
	}
}
```

마지막으로 메인 함수에서 사용한 Execute를 구현합니다.

## command

```go
package cmd
```

cmd라는 패키지를 만들어서 같은 패키지로 인식할 수 있게 합니다.

```go
import (
	"fmt"

	"github.com/spf13/cobra"
)
```

fmt와 cobra를 가져옵니다.

```go
func init() {
	rootCmd.AddCommand(version)
}
```

rootCmd에 아래의 version이라는 커맨드를 등록합니다.

```go
var version = &cobra.Command{
	Use:   "version",
	Short: "Print the version number",
	Long:  `Long text...`,
	Run: func(cmd *cobra.Command, args []string) {
		fmt.Println("Version v0.1")
	},
}
```

help로 접근하면 Available Commands에서 Use와 Short이 출력됩니다.

main.go version 라고 입력하면 Run에 작성한 코드대로 수행됩니다.