---
layout: post
title: Golang 키와 값으로 파일 저장하는 데이터베이스 NutsDB 라이브러리 알아보기
---

오늘은 Golang으로 키와 값 파일로 저장하는 NutsDB 라이브러리를 알아보려 합니다.

## NutsDB 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 NutsDB 패키지를 설치합니다.

```
go get -u github.com/xujiajun/nutsdb
```

홈의 go 폴더에 NutsDB 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/xujiajun/nutsdb"
)
```

fmt와 nutsdb를 가져옵니다.

```go
func main() {
	opt := nutsdb.DefaultOptions
	opt.Dir = "test_path/nutsdb_file"
	db, err := nutsdb.Open(opt)
	if err != nil {
		panic(err)
	}
```

nutsdb의 경로를 지정하고, DB 객체를 만듭니다.

```go
	if err := db.Update(
		func(tx *nutsdb.Tx) error {
			if err := tx.Put("test_bucket", []byte("test_key"), []byte("test_val"), 0); err != nil {
				return err
			}
			return nil
		}); err != nil {
		panic(err)
	}
```

버킷에 키와 값을 저장할 수 있습니다.

```go
	if err := db.View(
		func(tx *nutsdb.Tx) error {
			if e, err := tx.Get("test_bucket", []byte("test_key")); err != nil {
				return err
			} else {
				fmt.Println(string(e.Value))
			}
			return nil
		}); err != nil {
		panic(err)
	}
```

버킷의 키로 값을 조회할 수 있습니다.

```go
	if err := db.Update(
		func(tx *nutsdb.Tx) error {
			if err := tx.Delete("test_bucket", []byte("test_key")); err != nil {
				return err
			}
			return nil
		}); err != nil {
		panic(err)
	}
```

버킷의 키로 값을 삭제할 수 있습니다.

```go
	if err := db.View(
		func(tx *nutsdb.Tx) error {
			entries, err := tx.GetAll("test_bucket")
			if err != nil {
				return err
			}
			for _, entry := range entries {
				fmt.Println(string(entry.Key), string(entry.Value))
			}
			return nil
		}); err != nil {
		panic(err)
	}
```

버킷에 있는 값들을 모두 불러와서 조회할 수 있습니다.

```go
	err = db.Backup("backup")
	if err != nil {
		panic(err)
	}
```

현재 DB에 저장되있는 값들을 백업할 수 있습니다.

```go
	defer db.Close()
}
```

DB 객체를 정리합니다.