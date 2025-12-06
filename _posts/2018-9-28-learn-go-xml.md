---
layout: post
title: Go 언어 xml 대하여 배워보기 
---

오늘은 Go 언어의 xml 읽고 쓰기에 대하여 작성하면서 배워보려 합니다.

```go
package main
 
import (
    "encoding/xml"
    "fmt"
)
 
type test struct {
    Title   string
    Text    string
    Index	int
}
 
func main() {
    t := test{"hello,world!", "text line", 0}
    b, _ := xml.Marshal(t)
    s := string(b)
	fmt.Println(s)
}
```

xml 형식을 만들기 위한 구조체와 encoding/xml 패키지를 준비해줍니다.

구조체 필드에 값을 채운 변수에 Marshal 함수를 이용해 변환하면 xml 형식으로 출력됨을 확인할 수 있습니다.

```go
package main
 
import (
    "encoding/xml"
    "fmt"
)
 
type test struct {
    Title   string
    Text    string
    Index	int
}
 
func main() {
    d, _ := xml.Marshal(test{"hello,world!", "text line", 0})
    var t2 test
    xml.Unmarshal(d, &t2)
    fmt.Println(t2.Title,t2.Text, t2.Index)
}
```

이미 만들어진 xml에서 Unmarshal 함수를 이용해 구조체 필드로 옮겨 출력할 수 있습니다.