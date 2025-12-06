---
layout: post
title: Golang GitHub REST API v3 go-github 라이브러리 알아보기
---

오늘은 Golang으로 GitHub의 REST API v3를 구현한 go-github 라이브러리를 알아보려 합니다.

## go-github 설치

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

go get으로 go-github 패키지를 설치합니다.

```
go get github.com/google/go-github/github
```

홈의 go 폴더에 go-github 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"context"
	"fmt"

	"github.com/google/go-github/github"
)
```

context, fmt 그리고 github를 가져옵니다.

```go
func main() {
	client := github.NewClient(nil)
```

GitHub API 클라이언트를 생성합니다.

```go
	organizations, _, err := client.Organizations.List(context.Background(), "user_name", nil)
	if err != nil {
		panic(err)
	}
```

깃허브 사용자 이름을 적고, 해당 유저의 organization들을 조회합니다.

```go
	for _, organization := range organizations {
		option := &github.RepositoryListByOrgOptions{Type: "public"}
		repositories, _, err := client.Repositories.ListByOrg(context.Background(), organization.GetLogin(), option)
		if err != nil {
			panic(err)
		}
```

조회된 organization들을 반복해서 해당 organization의 저장소들을 가져옵니다.

```go
		for index, repository := range repositories {
			fmt.Println(index+1, repository.GetName())
		}
	}
}
```

반복하면서 저장소의 이름을 출력합니다.