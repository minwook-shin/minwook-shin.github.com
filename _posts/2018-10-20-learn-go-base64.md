---
layout: post
title: Go 언어 base64 알아보기
---

오늘은 Go 언어 기본 패키지에 포함되있는 encoding/base64을 이용하여 실습해보려 합니다.

```go
package main

import (
	"encoding/base64"
	"os"
	"fmt"
)
```

오늘의 포스팅 주제에 필요한 encoding/base64 패키지를 가져옵니다.

내용 출력을 위한 fmt 패키지와 os 패키지도 가져와줍니다.

```go
func main() {
	str := []byte("hello,world!")
	encoder := base64.NewEncoder(base64.StdEncoding, os.Stdout)
	encoder.Write(str)
	defer encoder.Close()
```

바이트 배열로 넣은 문자열을 base64로 인코딩해줍니다. os.Stdout로 인코딩된 값을 출력합니다.

Close로 반드시 닫아줍니다.

```go
	base64Str := "aGVsbG8sd29ybGQh"
	str, _ = base64.StdEncoding.DecodeString(base64Str)
	fmt.Printf("%q", str)
}
```

base64에서 디코딩해주기 위해 DecodeString를 사용합니다.

디코딩된 문자열을 출력해줍니다.

"hello,world!"를 base64로 인코딩하면 "aGVsbG8sd29ybGQh"가 되며, 다시 되돌리면 "hello,world!"로 되는 것을 확인할 수 있습니다.