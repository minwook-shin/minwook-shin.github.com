---
layout: post
title: Go 언어 파일 입출력 대하여 배워보기 
---

오늘은 Go 언어의 파일 입출력에 대하여 작성하면서 배워보려 합니다.

```go
package main

import (
	"fmt"
	"os"
)

func main() {
	data, _ := os.Create("test.txt")
	fmt.Fprintln(data,"파일에 쓰고 있습니다.")
	fmt.Fprintln(data,"Fprintln() 함수는 개행이 됩니다.")
	fmt.Fprintln(data,"Fprint(), Fprintf() 함수도 있습니다.")
	data.Close()
}
```

os 패키지의 create 함수에 파일 이름을 넣어주고, Fprintln 함수로 열어둔 파일에 텍스트를 작성해주는 코드입니다.

마지막에는 파일을 닫기 위해 Close를 호출합니다.

```go
package main

import (
	"fmt"
	"os"
)

func main() {
	var t string
	read, _ := os.Open("test.txt")
	for i := 0; i < 11; i++ {
		fmt.Fscan(read,&t)
		fmt.Print(t," ")
	}
	read.Close()
}
```

아까 작성한 파일을 Open 함수로 열어 for문으로 반복하면서 파일을 읽어내립니다.

마지막에는 아까와 동일하게 Close 함수로 파일을 닫아줍니다.