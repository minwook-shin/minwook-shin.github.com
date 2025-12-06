---
layout: post
title: Golang BoltDB 툴킷 Storm 라이브러리 알아보기 2
---

오늘도 Golang으로 BoltDB 툴킷을 구현한 Storm 라이브러리를 알아보려 합니다.

## Storm 설치

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

go get으로 Storm 패키지를 설치합니다.

```
go get -u github.com/asdine/storm
```

홈의 go 폴더에 Storm 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"
	"time"

	"github.com/asdine/storm"
)
```

fmt, time 그리고 storm을 가져옵니다.

```go
type testInfo struct {
	ID        int       `storm:"id,increment"`
	Email     string    `storm:"unique"`
	CreatedAt time.Time `storm:"index"`
}
```

구조체를 생성합니다.

increment로 따로 지정안해도 순서대로 값이 증가하고, unique로 값이 같지 않게 할 수 있습니다.

```go
func main() {
	db, err := storm.Open("test.db")
	if err != nil {
		panic(err)
	}
	defer db.Close()
```

test.db라는 파일을 기반으로 데이터베이스를 열고, defer 키워드로 데이터베이스를 닫습니다.

```go
	err = db.Update(&testInfo{ID: 1, Email: "new@example.com"})
	if err != nil {
		fmt.Println(err)
	}
```

객체를 기반으로 데이터베이스에서 업데이트합니다.

```go
	var testList []testInfo
	err = db.All(&testList)
	if err != nil {
		fmt.Println(err)
	}
	fmt.Println(testList)
```

버킷의 모든 레코드를 같이 가져옵니다.

```go
	err = db.DeleteStruct(&test1)
	if err != nil {
		fmt.Println(err)
	}
```

버킷의 모든 레코드를 삭제합니다.

```go
	err = db.All(&testList)
	if err != nil {
		fmt.Println(err)
	}
	fmt.Println(testList)
}
```

삭제한 내용을 확인하려고 다시 버킷의 모든 레코드를 같이 가져옵니다.