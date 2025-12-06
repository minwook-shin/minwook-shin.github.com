---
layout: post
title: Golang Git 구현 go-git 라이브러리 알아보기
---

오늘은 Golang으로 Git을 구현한 go-git 라이브러리를 알아보려 합니다.

## go-git 설치

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

go get으로 go-git 패키지를 설치합니다.

```
go get -u gopkg.in/src-d/go-git.v4
```

홈의 go 폴더에 go-git 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"os"

	"gopkg.in/src-d/go-git.v4"
)
```

fmt, os 그리고 go-git을 가져옵니다.

```go
func main() {
	_, err := git.PlainClone("test", false, &git.CloneOptions{
		URL:      "https://github.com/github/gitignore",
		Progress: os.Stdout,
	})
	if err != nil {
		fmt.Println(err)
	}
```

특정 URL로 저장소를 클론하여 가져옵니다.

이 때 CloneOptions에 의하여 표준출력으로 진행도가 표시됩니다.

이미 존재하는 폴더에 클론을 받으면 repository already exists라는 오류가 출력됩니다.

```go
	repository, err := git.PlainOpen("test")
	if err != nil {
		fmt.Println(err)
	}
```

주어진 경로의 깃 폴더를 엽니다.

```go
	worktree, err := repository.Worktree()
	if err != nil {
		panic(err)
	}
```

작업 트리를 생성합니다.

```go
	err = worktree.Pull(&git.PullOptions{RemoteName: "origin"})
	if err != nil {
		fmt.Println(err)
	}
```

작업 트리를 기반으로 Pull 작업을 수행하여 원격 저장소의 변경점을 가져옵니다.

이미 최신 상태라면 already up-to-date라는 오류가 출력됩니다.

```go
	reference, err := repository.Head()
	if err != nil {
		panic(err)
	}
	commit, err := repository.CommitObject(reference.Hash())
	if err != nil {
		panic(err)
	}
	fmt.Println(commit)
```

HEAD의 커밋을 반환하여 최근 커밋 메시지만 확인합니다.