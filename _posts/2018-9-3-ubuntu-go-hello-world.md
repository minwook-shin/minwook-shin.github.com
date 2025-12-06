---
layout: post
title: 우분투에서 go 언어 hello world 코딩하고 컴파일 해보기
---

오늘은 우분투 18.04에서 go 언어로 hello, world를 출력해보려 합니다.

이 포스팅에서는 우분투와 비주얼스튜디오 코드가 사용되었으나, 윈도우에서도 코드는 동일합니다.

## 우분투에서 컴파일러 설치하기

고 언어 위키 https://github.com/golang/go/wiki/Ubuntu 에 따르면

```bash
sudo apt-get install golang-go
```

위와 같이 안내하고 있습니다.

만약 버전이 낮다면, ppa를 등록해서 하는 방식을 추천하고 있습니다.


```bash
$ sudo add-apt-repository ppa:gophers/archive
$ sudo apt-get update
$ sudo apt-get install golang-1.10-go
```

물론 최신버전의 go 언어를 스냅으로도 설치할 수 있습니다.

```bash
sudo snap install --classic go
```

## IDE

IDE는 비주얼스튜디오 코드와 Go for Visual Studio Code 확장을 설치하면 됩니다.

IntelliSense, Code Navigation, Code Editing, Diagnostics, Testing, Debugging 등을 지원합니다.

플러그인을 설치하면 gocode, gopkgs, go-outline, go-symbols, guru, gorename, dlv, godef, godoc, goreturns, golint 패키지를 설치하라고 팝업 안내 창이 나옵니다.


## 파일 생성

작업 폴더에 src라는 소스 폴더를 만들고, 자신이 원하는 패키지 이름의 폴더에 go 확장자로 생성합니다.

예시 경로로 ```go/src/helloworld```가 될 수 있습니다.

## 코딩

```go
package main

import "fmt"	

func main() {
	fmt.Print("hello world")
}
```

그럼 간단하게 hello, world를 출력해보겠습니다.

모든 go 언어의 파일은 패키지가 먼저 서술되어야 하며, fmt 패키지를 메인 함수에서 출력할 때를 위하여 가져옵니다.

그리고 메인 함수에서는 "hello world"를 출력해줍니다.

## 컴파일

```
go install hello 
```

소스 파일에서 패키지 폴더의 이름을 가지고 설치해주면, 해당 작업 폴더의 bin 폴더에서 hello라는 바이너리 파일이 있는 것을 확인할 수 있습니다.

## 바로 실행하기

```
go run hello/helloworld.go 
```

hello 폴더의 helloworld go 파일로 바로 실행할 수 있습니다.