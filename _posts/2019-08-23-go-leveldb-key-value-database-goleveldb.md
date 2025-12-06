---
layout: post
title: Golang levelDB 구현 goleveldb 라이브러리 알아보기
---

오늘은 Golang으로 키와 값 저장하는 levelDB 구현한 goleveldb 라이브러리를 알아보려 합니다.

## goleveldb 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 goleveldb 패키지를 설치합니다.

```
go get github.com/syndtr/goleveldb/leveldb
```

홈의 go 폴더에 goleveldb 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/syndtr/goleveldb/leveldb"
	"github.com/syndtr/goleveldb/leveldb/filter"
	"github.com/syndtr/goleveldb/leveldb/opt"
)
```

fmt와 leveldb를 가져옵니다.

```go
func decode(b []byte) string {
	return string(b[:len(b)])
}
```

바이트 슬라이스를 문자열로 바꿔주는 함수를 미리 만듭니다.

```go
func main() {
	db, err := leveldb.OpenFile("test_path/test_file", nil)
	if err != nil {
		panic(err)
	}
```

인자로 받은 경로를 바탕으로 leveldb 파일을 생성합니다.

```go
	err = db.Put([]byte("key"), []byte("value"), nil)
	data, err := db.Get([]byte("key"), nil)
	fmt.Println(decode(data))
```

키와 값을 파일에 저장하여 키로 값을 찾습니다.

미리 만들어둔 decode 함수로 저장된 바이트를 문자열로 바꾸어 출력해줍니다.

```go
	iter := db.NewIterator(nil, nil)
	for iter.Next() {
		key := iter.Key()
		value := iter.Value()
		fmt.Println(key, decode(value))
	}
	iter.Release()
	err = iter.Error()
```

데이터베이스의 값들을 iterator로 받아서 for문에 사용할 수 있습니다.

Release로 iterator를 해제하면 마무리됩니다.

```go
	err = db.Delete([]byte("key"), nil)
```

Delete로 키와 값을 삭제할 수 있습니다.

```go
	batch := new(leveldb.Batch)
	batch.Put([]byte("key1"), []byte("value1"))
	batch.Put([]byte("key2"), []byte("value2"))
	batch.Delete([]byte("key1"))
	err = db.Write(batch, nil)
	defer db.Close()
```

여러 개의 키와 값을 일괄로 처리할 수 있습니다.

```go
	o := &opt.Options{
		Filter: filter.NewBloomFilter(10),
	}
	db, err = leveldb.OpenFile("test_path/test_BloomFilter", o)
	if err != nil {
		panic(err)
	}

	defer db.Close()
}
```

bloom filter를 사용할 수 있습니다.