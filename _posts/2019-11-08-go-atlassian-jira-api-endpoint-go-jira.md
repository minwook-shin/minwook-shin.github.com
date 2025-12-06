---
layout: post
title: Golang JIRA API go-jira 라이브러리 알아보기
---

오늘은 Golang으로 Atlassian사의 JIRA를 API 구현한 go-jira 라이브러리를 알아보려 합니다.

## go-jira 설치

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

go get으로 go-jira 패키지를 설치합니다.

```
go get github.com/andygrunwald/go-jira
```

홈의 go 폴더에 go-jira 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"crypto/tls"
	"fmt"
	"net/http"

	"github.com/andygrunwald/go-jira"
)
```

crypto, fmt, net 그리고 go-jira 패키지를 가져옵니다.

```go
func main() {
	transport := &http.Transport{TLSClientConfig: &tls.Config{InsecureSkipVerify: true}}
	client := &http.Client{Transport: transport}
```

TLS 구성을 변경하는 설정과 함께 클라이언트를 생성합니다.

```go
	jiraClient, _ = jira.NewClient(client, "https://issues.apache.org/jira/")
	issue, _, _ = jiraClient.Issue.Get("MESOS-1", nil)
```

JIRA API 클라이언트를 생성하고 특정 JIRA 이슈를 가져옵니다.

```go
	fmt.Println(issue.Key)
	fmt.Println(issue.Fields.Summary)
	fmt.Println(issue.Fields.Type.Name)
	fmt.Println(issue.Fields.Priority.Name)
```

특정 JIRA 이슈를 가져오면 출력할 수 있습니다.

```go
	jiraClient, _ = jira.NewClient(nil, "https://jira.atlassian.com/")
	request, _ := jiraClient.NewRequest("GET", "/rest/api/2/project", nil)
```

JIRA API 클라이언트로 API 요청을 생성합니다.

```go
	projects := new([]jira.Project)
	_, err := jiraClient.Do(request, projects)
	if err != nil {
		panic(err)
	}

	for _, project := range *projects {
		fmt.Println(project.Key, project.Name)
	}
}
```

API 요청으로 받은 응답을 받고 반복하여 출력합니다.