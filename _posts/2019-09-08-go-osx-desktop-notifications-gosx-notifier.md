---
layout: post
title: Golang OSX 데스크톱 알림 gosx-notifier 라이브러리 알아보기
---

오늘은 Golang으로 OSX 데스크톱에 알림을 푸시할 수 있는 gosx-notifier 라이브러리를 알아보려 합니다.

## gosx-notifier 설치

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

go get으로 gosx-notifier 패키지를 설치합니다.

```
go get github.com/deckarep/gosx-notifier
```

홈의 go 폴더에 gosx-notifier 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	gosxnotifier "github.com/deckarep/gosx-notifier"
)
```

gosx-notifier를 가져옵니다.

```go
func main() {
	note := gosxnotifier.NewNotification("메시지")
```

OSX에서 띄울 수 있는 메시지 알림을 생성합니다.

```go
	note.Title = "제목"
	note.Subtitle = "부제목"
```

제목과 부제목을 설정할 수 있습니다.

```go
	note.Sound = gosxnotifier.Default
```

소리는 아래와 같은 목록으로 설정할 수 있습니다.

* default
* Basso
* Blow
* Bottle
* Frog
* Funk
* Glass
* Hero
* Morse
* Ping
* Pop
* Purr
* Sosum
* Submarine
* Tink

```go
	note.Group = "com.example.app.identifier"
```

그룹을 만들어서 각자의 알림끼리 묶이게 할 수 있습니다.

```go
	//note.Sender = "com.apple.Safari"
	note.Link = "https://minwook-shin.github.io"
```

Sender를 Safari로 해서 브라우저를 열게 하거나, 링크를 지정하여 알림을 클릭했을 때에 해당 리크를 열게 할 수 있습니다.

```go
	note.AppIcon = "original.png"
	note.ContentImage = "original.png"
```

알림에 들어가는 이미지도 설정할 수 있습니다.

```go
	err := note.Push()
	if err != nil {
		panic(err)
	}
}
```

푸시하면 자신의 컴퓨터에 우측 상단에 알림이 표시됩니다.