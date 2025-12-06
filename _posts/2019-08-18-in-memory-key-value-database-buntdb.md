---
layout: post
title: Golang 인 메모리 데이터베이스 BuntDB 라이브러리 알아보기
---

오늘은 Golang으로 인덱싱을 지원하는 인 메모리 데이터베이스 BuntDB 라이브러리를 알아보려 합니다.

## BuntDB 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 BuntDB 패키지를 설치합니다.

```
go get -u github.com/tidwall/buntdb
```

홈의 go 폴더에 BuntDB 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/tidwall/buntdb"
)
```

fmt와 buntdb를 가져옵니다.

```go
func main() {
	db, err := buntdb.Open("data.db")
```

지정된 파일에 데이터베이스를 지정합니다.

```go
	db, err := buntdb.Open(":memory:")
	if err != nil {
		panic(err)
	}
```

혹은 메모리로 지정할 수 있습니다.

```go
	err = db.Update(func(tx *buntdb.Tx) error {
		_, _, err = tx.Set("first_key", "hello", nil)
		_, _, err = tx.Set("second_key", "world", nil)
		return nil
	})
```

읽고 쓰는 트랜잭션에서 업데이트를 수행할 수 있습니다.

키를 기준으로 데이터를 넣게 됩니다.

```go
	err = db.View(func(tx *buntdb.Tx) error {
		val, err := tx.Get("first_key")
		if err != nil {
			return err
		}
		fmt.Printf("%s\n", val)
		return nil
	})
```

읽기만 가능한 트랙잭션에서 해당 키에 대한 값을 출력합니다.

```go
	err = db.View(func(tx *buntdb.Tx) error {
		err = tx.Ascend("", func(key, value string) bool {
			fmt.Printf("%s, %s\n", key, value)
			return true
		})
		return err
	})
```

iterator로 모든 항목을 가져올 수 있습니다.

```go
	db.CreateIndex("tests", "test:*", buntdb.IndexString)
	err = db.Update(func(tx *buntdb.Tx) error {
		_, _, err = tx.Set("test:0:tests", "fisrt_test", nil)
		_, _, err = tx.Set("production:1:tests", "real", nil)
		_, _, err = tx.Set("test:2:tests", "second_test", nil)
		return nil
	})
```

새로운 인덱스를 만들고 해당 인덱스에 맞는 키와 값들을 추가합니다.

```go
	db.View(func(tx *buntdb.Tx) error {
		tx.Ascend("tests", func(key, val string) bool {
			fmt.Printf("%s %s\n", key, val)
			return true
		})
		return nil
	})
```

미리 만들어둔 인덱스를 이용하여 iterator로 원하는 항목들을 가져올 수 있습니다.

```go
	defer db.Close()
}
```

데이터베이스 트랜잭션을 닫습니다.