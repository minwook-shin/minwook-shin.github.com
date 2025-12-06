---
layout: post
title: Golang 웹 요청 유효성 규칙 검사 govalidator 라이브러리 알아보기
---

오늘은 Golang으로 웹의 요청으로 유효성 규칙을 검사하는 govalidator 라이브러리를 알아보려 합니다.

## govalidator 설치

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

go get으로 govalidator 패키지를 설치합니다.

```
go get github.com/thedevsaddam/govalidator
```

홈의 go 폴더에 govalidator 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"encoding/json"
	"net/http"

	"github.com/thedevsaddam/govalidator"
)
```

encoding, net 그리고 govalidator를 가져옵니다.

```go
func main() {
	http.HandleFunc("/", handler)
	http.ListenAndServe(":8000", nil)
}
```

핸들러 함수를 만들어서 8000 포트가 열린 서버를 구동합니다.

```go
func handler(w http.ResponseWriter, r *http.Request) {
	rules := govalidator.MapData{
		"required_text": []string{"required", "between:4,8"},
		"email":         []string{"email"},
		"url":           []string{"url"},
		"phone":         []string{"digits:11"},
		"date":          []string{"date"},
		"boolean":       []string{"bool"},
	}
```

govalidator로 유효성을 검사할 요청 데이터에 대한 규칙 구조를 작성합니다.

```go
	messages := govalidator.MapData{
		"required_text": []string{"required:required TEXT"},
	}
```

govalidator에서 규칙에 대한 출력 메시지 구조를 작성합니다.

```go
	options := govalidator.Options{
		Request:         r,
		Rules:           rules,
		Messages:        messages,
		RequiredDefault: true,
	}
```

요청 데이터에 대한 유효성 검사를 진행할 때 설정을 지정합니다.

RequiredDefault를 참으로 설정하면 모든 요청 데이터가 반드시 필요하게 됩니다.

```go
	validator := govalidator.New(options)
	values := validator.Validate()
	err := map[string]interface{}{"ERROR": values}
```

validator 객체를 만들어서 받아오는 쿼리로 데이터를 유효성 검사합니다.

```go
	w.Header().Set("Content-type", "application/json")
	json.NewEncoder(w).Encode(err)
}
```

헤더를 설정하고 json으로 인코딩합니다.