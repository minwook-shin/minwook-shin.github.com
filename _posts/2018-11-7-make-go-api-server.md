---
layout: post
title: Go 언어 gorilla/mux 사용하여 rest api 만들어보기
---

오늘은 Go 언어 gorilla/mux를 이용하여 rest api를 만드는 실습해보려 합니다.

우선 rest api를 만드려면 get, post, put, delete를 구현해야 합니다.

이번 실습에서는 여러 데이터들 중에 개별적으로 JSON으로 출력하는 get만 구현해보도록 하겠습니다.

## 사전 설정

```bash
$ go get github.com/gorilla/mux
```

라우터를 생성하기 위해 고언어의 외부 라이브러리인 gorilla/mux를 사용합니다.

## 패키지 가져오기

```go
package main

import (
    "net/http"
    "github.com/gorilla/mux"
    "encoding/json"
    "log"
)
```

main 파일에 import를 하여 웹서버를 만들 준비를 합니다.

## 메인 함수 준비

```go
func main() {
    router := mux.NewRouter()
    http.ListenAndServe(":8080", router)
}
```

라우터를 만들어주고, 해당 라우터를 가지고 자신의 아이피+8080 포트에 서버를 구동합니다.

```go
func httpHandler(handler http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		log.Print(r.RemoteAddr," ", r.Proto, " ", r.Method, " ", r.URL)
		handler.ServeHTTP(w, r)
	})
}
```

서버를 구동하고 log를 출력하거나, 기타 작업을 수행하기 위해 핸들러를 만들어서 반환할 수 있습니다.

```go
http.ListenAndServe(":8080", httpHandler(router))
```

그리고나서 메인 함수에서 라우터를 위에서 구현한 함수에 넣어주면 됩니다.

## 라우터 함수 등록

```go
func main() {
    router := mux.NewRouter()
    router.HandleFunc("/test/{code}", GetData).Methods("GET")
    http.ListenAndServe(":8080", router)
}
```

GetData라는 함수를 GET 기능을 하는 /test/{code} 라우터에 등록합니다.

이 함수를 잘 구현하기만 하면, 자신의 아이피+8080포트+/test+{번호}로 접속하여 해당 번호의 데이터를 json으로 반환할 수 있습니다.

## 구조체 구현

```go
type data struct{
	code string
	Title string
	Description string
}

var testData []data
```

접속하는 코드에 따라 보여주는 내용이 달라야하므로 데이터를 구별하는 code를 넣어주고, 데이터를 구분하기 위해 간단히 제목과 설명으로 나누어놨습니다.

## 데이터 삽입

```go
testData = append(testData, data{code: "1", Title: "fist title", Description: "desc"}})
testData = append(testData, data{code: "2", Title: "second title", Description: "desc"}})
```

데이터를 append하여 추가해줍니다.

향후에는 이를 csv파일에서 가져오거나, 웹 서버에서 가져오게 할 수 있습니다.

## 함수 구현 

```go
func GetData(w http.ResponseWriter, r *http.Request) {}
```

우리는 위 함수를 구현해주어야 합니다.

```go
func GetData(w http.ResponseWriter, r *http.Request) {
    p := mux.Vars(r)
	for _, i := range testData {
		if i.code == p["code"] {
			json.NewEncoder(w).Encode(i)
			return
		}
	}
	json.NewEncoder(w).Encode(&event{})
}
```

for range로 해당 코드를 가진 부분만 인코드해주고 반환합니다.

## 기타 

삽입은 json을 디코드해주고 슬라이스에서 append로 추가해주면 됩니다.

삭제는 get과 마찬가지로 순회하고 삭제할 이전과 이후의 위치를 슬라이싱해주면 됩니다.