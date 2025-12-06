---
layout: post
title: Go 언어 json 대하여 배워보기 
---

오늘은 Go 언어의 json 읽고 쓰기에 대하여 작성하면서 배워보려 합니다.

```go
package main
 
import (
    "encoding/json"
    "fmt"
)
 
// test struct
type test struct {
    Title   string
    Text    string
    Index   int 
}
 
func main() {
    s := test{"hello", "hello world, go!", 0}
    b, _ := json.Marshal(s)
    str := string(b)
    fmt.Println(str)
}
```

test라는 구조체를 만들어서 변수를 구현합니다.

해당 변수를 json으로 변환하기 위해 Marshal 함수를 이용해줍니다.

출력하기 위해 string으로 변환하고 출력해줍니다.

```go
package main
 
import (
    "encoding/json"
    "fmt"
)

// test2 struct
type test2 struct {
    Title   string `json:"title"`
    Text    string `json:"txt"`
    Index   int `json:"idx"`
}
 
func main() {
    s2 := test2{"hello", "hello world, go!", 0}
    b2, _ := json.Marshal(s2)
    str2 := string(b2)
    fmt.Println(str2)
}
```

처음 예제와 비슷하지만, json에 표기될 키값의 이름을 바꿀 수 있습니다.

```go
package main
 
import (
    "encoding/json"
    "fmt"
)
 
// test struct
type test struct {
    Title   string
    Text    string
    Index   int 
}

func main() {
    b3, _ := json.Marshal(test{"title", "hello world! go!",0})
    var str3 test
    json.Unmarshal(b3, &str3)
    fmt.Println(str3.Title, str3.Text, str3.Index)
}
```

읽어들인 json을 문자열로 해독해서 각각의 구조체의 필드로 데이터를 조회할 수 있습니다.

> update : 10월 2일자로 오타를 수정하였습니다. 알려주셔서 감사합니다.