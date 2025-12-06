---
layout: post
title: Golang stats handler stats 라이브러리 알아보기
---

오늘은 Golang으로 어플리케이션의 stats handler인 stats 라이브러리를 알아보려 합니다.

## stats 설치

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

go get으로 stats 패키지를 설치합니다.

```
go get github.com/thoas/stats
```

홈의 go 폴더에 stats 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"encoding/json"

	"github.com/thoas/stats"
	"net/http"
)
```

encoding, net 그리고 stats를 가져옵니다.

```go
func main() {
	h := http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		middleware := stats.New()
		stats := middleware.Data()
```

HTTP 핸들러 함수에사 통계 데이터를 데이터 직렬화 가능한 구조로 생성합니다.

```go
		byteStats, err := json.Marshal(stats)
		if err != nil {
			http.Error(w, err.Error(), http.StatusInternalServerError)
		}
```

JSON 인코딩으로 바이트를 반환합니다.

```go
		w.Header().Set("Content-Type", "application/json")
		w.Write(byteStats)
	})
```

헤더와 만들어둔 통계 바이트를 웹으로 출력합니다. 

```go
	handler := stats.New().Handler(h)
```

미들웨어 인터페이스를 구현한 핸들러를 생성합니다.

```go
	http.ListenAndServe(":8080", handler)
}
```

해당 핸들러를 기반해서 8080 포트로 서버를 구동합니다.