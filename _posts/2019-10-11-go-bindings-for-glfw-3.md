---
layout: post
title: Golang Graphics Library Framework GLFW 라이브러리 알아보기
---

오늘은 Golang으로 포팅된 Graphics Library 프레임워크인 GLFW 3.2 라이브러리를 알아보려 합니다.

## GLFW 설치

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

go get으로 GLFW 패키지를 설치합니다.

```
go get -u github.com/go-gl/glfw/v3.2/glfw
```

홈의 go 폴더에 GLFW 소스코드와 패키지 파일이 생성됩니다.

우분투에서는 libgl1-mesa-dev와 xorg-dev 패키지, 맥에서는 Command Line Tools가 설치되어있어야 합니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"runtime"

	"github.com/go-gl/glfw/v3.2/glfw"
)
```

runtime과 glfw를 가져옵니다.

```go
func init() {
	runtime.LockOSThread()
}
```

고루틴을 현재 OS의 스레드에 연결합니다

```go
func main() {
	err := glfw.Init()
	if err != nil {
		panic(err)
	}
```

GLFW 라이브러리를 초기화합니다

```go
	defer glfw.Terminate()
```

defer 키워드로 남아있는 모든 창을 지우고, 자원을 모두 해제하면서 초기화합니다.

```go
	window, err := glfw.CreateWindow(640, 320, "title", nil, nil)
	if err != nil {
		panic(err)
	}
```

특정 크기의 윈도우 컨텍스트를 만듭니다.

```go
	window.MakeContextCurrent()
```

컨텍스트로 창을 구현합니다.

```go
	for !window.ShouldClose() {
		window.SwapBuffers()
		glfw.PollEvents()
	}
}

```

창이 닫히지 않을 때 계속 반복적으로 창의 버퍼를 스왑하고, 이벤트와 관련된 윈도우와 입력에 대한 콜백을 호출합니다.