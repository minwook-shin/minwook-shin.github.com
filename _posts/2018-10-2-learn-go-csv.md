---
layout: post
title: Go 언어 csv 대하여 배워보기 
---

오늘은 Go 언어에서 csv파일을 읽고 쓰는 방법에 대하여 작성하면서 배워보려 합니다.

```go
package main

import (
	"bufio"
	"encoding/csv"
	"os"
)

func main() {
	w, _ := os.Create("DB.csv")
	wr := csv.NewWriter(bufio.NewWriter(w))
	wr.Write([]string{"A", "1"})
	wr.Write([]string{"B", "2"})
	wr.Flush()
	w.Close()
}
```

DB라는 이름을 가진 csv파일을 만들기 위해 NewWriter 함수로 레코드를 쓸 수 있게 만들어줍니다.

작성하고 마지막으로 파일을 닫아줍니다.

```go
package main

import (
	"encoding/csv"
	"fmt"
	"os"
)

func main() {
	r, _ := os.Open("DB.csv")
	read := csv.NewReader(r)
	read.FieldsPerRecord = -1
	data, _ := read.ReadAll()
	for _, e := range data {
		fmt.Println(e[0] + " " + e[1])
	}
	r.Close()
}
```

DB라는 이름을 가진 csv파일을 열기 위해 Open 함수로 열어줍니다.

전부 읽어들여서 for range문으로 각 필드들을 나누어줍니다.