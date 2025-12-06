---
layout: post
title: Golang SQL 드라이버 Mock go-sqlmock 라이브러리 알아보기
---

오늘은 Golang으로 SQL 드라이버로 Mock 객체를 만들어서 데이터베이스를 테스트하는 go-sqlmock 라이브러리를 알아보려 합니다.

## go-sqlmock 설치

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

go get으로 go-sqlmock 패키지를 설치합니다.

```
go get github.com/DATA-DOG/go-sqlmock
```

홈의 go 폴더에 go-sqlmock 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package test
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언하지 않고 테스트를 위해 다른 이름을 적습니다.

```go
import (
	"database/sql"
	"testing"

	"github.com/DATA-DOG/go-sqlmock"
)
```

sql과 testing 그리고 sqlmock을 가져옵니다.

```go
func dbInsert(db *sql.DB, id, count int64) (err error) {
	tx, err := db.Begin()
	if err != nil {
		return
	}
```

transaction을 기작합니다.

```go
	defer func() {
		switch err {
		case nil:
			err = tx.Commit()
		default:
			tx.Rollback()
		}
	}()
```

오류나는 상황에 따라 커밋하거나 롤백합니다.

```go
	if _, err = tx.Exec("UPDATE entity SET views = views"); err != nil {
		return
	}
	if _, err = tx.Exec("INSERT INTO entity (id, count) VALUES (?, ?)", id, count); err != nil {
		return
	}
	return
}
```

UPDATE와 INSERT INTO로 데이터를 삽입하여 테스트할 준비를 합니다.

```go
func TestDBUpdate(t *testing.T) {
	db, mock, err := sqlmock.New()
	if err != nil {
		t.Error(err)
	}
	defer db.Close()
```

테스트 함수를 만들고, sqlmock 데이터베이스 연결 객체를 생성합니다.

```go
	mock.ExpectBegin()
	mock.ExpectExec("UPDATE entity").WillReturnResult(sqlmock.NewResult(1, 1))
	mock.ExpectExec("INSERT INTO entity").WithArgs(1, 10).WillReturnResult(sqlmock.NewResult(1, 1))
	mock.ExpectCommit()
```

Exec()로 쿼리를 호출하는 것과 같은 효과가 나게 테스트를 진행합니다.

```go
	if err = dbInsert(db, 1, 10); err != nil {
		t.Error(err)
	}
```

실제 DB로 삽입되는 함수를 사용합니다.

```go
	if err := mock.ExpectationsWereMet(); err != nil {
		t.Error(err)
	}
}
```

sql문이 조건에 충족되었는지 확인합니다.
