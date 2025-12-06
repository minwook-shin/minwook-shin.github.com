---
layout: post
title: Golang json 쿼리 ORM GoJSONQ 라이브러리 알아보기
---

오늘은 Golang으로 간단한 json 데이터를 쿼리하는 ORM 라이브러리인 GoJSONQ 라이브러리를 알아보려 합니다.

## GoJSONQ 설치

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

go get으로 GoJSONQ 패키지를 설치합니다.

```
go get gopkg.in/thedevsaddam/gojsonq.v2
```

홈의 go 폴더에 GoJSONQ 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"gopkg.in/thedevsaddam/gojsonq.v2"
)
```

fmt와 gojsonq를 가져옵니다.

```go
func main() {
	const text = `{"entity":[
		{"id":"first",
		"description":[
			{"name":"golang","score":90},
			{"name":"java","score":50}]}
			]}`

	jsonSTR := gojsonq.New().JSONString(text)
```

json 텍스트를 기반으로 JSONQ 객체를 생성합니다.

```go
	id := jsonSTR.Find("entity.[0].id")
	fmt.Println(id)

	name := jsonSTR.Reset().Find("entity.[0].description.[0].name")
	fmt.Println(name)
}
```

지정한 경로의 내용을 찾아서 반환하며 출력합니다.

그 외에도 [해당 링크](https://github.com/thedevsaddam/gojsonq/wiki/Queries)에 다양한 쿼리들이 많이 있으므로 찾아서 사용할 수 있습니다.