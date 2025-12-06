---
layout: post
title: Go 언어 http get 대하여 배워보기 
---

오늘은 Go 언어의 http get에 대하여 작성하면서 배워보려 합니다.

```go
package main
 
import (
    "fmt"
    "io/ioutil"
    "net/http"
)
 
func main() {
    page, _ := http.Get("http://info.cern.ch/")
    data, _ := ioutil.ReadAll(page.Body)
	fmt.Println(string(data))
	page.Body.Close()
}
```

http.Get으로 홈페이지의 소스를 가져올 수 있습니다.

마지막에는 반드시 Close로 닫아줍니다.

```go
package main
 
import (
    "fmt"
    "io/ioutil"
    "net/http"
)
 
func main() {
    req, _ := http.NewRequest("GET", "http://info.cern.ch/", nil)
    client := &http.Client{}
    resp, _ := client.Do(req)
    bytes, _ := ioutil.ReadAll(resp.Body)
    str := string(bytes)
	fmt.Println(str)
	resp.Body.Close()
}
```

Request 스트림을 추가할 때에는 Request 객체를 직접 생성해서 이를 통해 호출합니다.