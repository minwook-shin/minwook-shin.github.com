---
layout: post
title: Go 언어 hex 알아보기
---

오늘은 Go 언어 기본 패키지에 포함되있는 encoding/hex를 이용하여 실습해보려 합니다.

```go
package main

import (
	"encoding/hex"
	"fmt"
)
```

오늘의 포스팅 주제에 필요한 encoding/hex 패키지를 가져옵니다.

내용 출력을 위한 fmt 패키지도 가져와줍니다.

```go
func main() {
	source := []byte("Hello world!")
	encodeText := make([]byte, hex.EncodedLen(len(source)))
	hex.Encode(encodeText, source)
    fmt.Println(string(encodeText))
```

문자열로 채운 바이트 배열과 make로 만들어둔 빈 바이트 배일을 만들어줍니다.

make로 만들 때에는 hex로 인코딩한 길이로 만들어두어야 합니다.

hex.Encode()으로 인코딩해주고, 출력해줍니다.

```go
	source = make([]byte, hex.DecodedLen(len(encodeText)))
	hex.Decode(source, encodeText)
	fmt.Println(string(source[:]))
}
```

다시 hex로 인코딩된 텍스트를 디코드해주어야 합니다.

make로 해당 인코딩된 텍스트가 디코드되었을 때에 나올 길이를 지정하며 바이트 배열을 생성해줍니다.

디코드해주고 string으로 출력해줍니다.