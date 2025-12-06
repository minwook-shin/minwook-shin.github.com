---
layout: post
title: Golang Struct과 Map 병합 Mergo 라이브러리 알아보기
---

오늘은 Golang으로 Struct과 Map들을 병합할 수 있는 Mergo 라이브러리를 알아보려 합니다.

## Mergo 설치

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

go get으로 Mergo 패키지를 설치합니다.

```
go get github.com/imdario/mergo
```

홈의 go 폴더에 Mergo 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/imdario/mergo"
)
```

fmt, mergo를 가져옵니다.

```go
type User struct {
	ID       string
	Password string
}
```

병합할 대상의 구조체를 만들어둡니다.

```go
func main() {
	newInfo := User{
		ID:       "user_id_1",
		Password: "0000",
	}
	oldInfo := User{
		ID: "user_id_2",
	}
```

원본 객체와 병합할 때 새로 넣을 객체를 생성합니다.

```go
	mergo.Merge(&oldInfo, newInfo)
	fmt.Println(oldInfo)
```

두 객체를 병합합니다.

```go
	mergo.Merge(&oldInfo, newInfo, mergo.WithOverride)
	fmt.Println(oldInfo)
```

가존 값이 비어있지 않다면 새로운 값으로 덮어씌우면서 두 객체를 병합합니다.
