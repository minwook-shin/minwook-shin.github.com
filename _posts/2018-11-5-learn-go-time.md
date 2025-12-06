---
layout: post
title: Go 언어 time 알아보기
---

오늘은 Go 언어 기본 패키지에 포함되있는 time을 이용하여 시간을 간단히 출력해보려 합니다.

```go
package main

import (
	"fmt"
	"time"
)
```

time 패키지로 시간을 구하고, fmt 패키지로 구한 시간을 출력합니다.

```go
func main() {
	timeNow := time.Now()
	fmt.Println(timeNow)
```

자신의 위치를 기반한 현재 시간이 출력됩니다.

```go
	fmt.Println(timeNow.Location())
	fmt.Println(timeNow.Weekday())
```

Local로 위치가 표기되며, 날짜도 출력할 수 있습니다.

```go
	timeCustom := time.Date(2017, 12, 22, 12, 00, 00, 000000000, time.FixedZone("Asia/Seoul", 9*60*60))
	fmt.Println(timeCustom)
```

시간을 임의대로 구조체 형식으로 넣어서 변경된 시간을 만들 수 있습니다.

```go
	fmt.Println(timeCustom.Location())
	fmt.Println(timeCustom.Weekday())
```

구조체로 만들어진 시간도 현재 시간과 동일하게 메소드를 사용할 수 있습니다.

```go
	fmt.Println(timeCustom.Equal(timeNow))
```

현재 시간과 수정한 시간이 같은지 boolean으로 판별할 수 있습니다.

```go
	diffTime := timeNow.Sub(timeCustom)
	fmt.Println(diffTime)
```

구조체로 넣은 시간과 현재 시간의 차이를 구할 수 있습니다.

```go
	fmt.Println(timeCustom.Add(diffTime))
	fmt.Println(timeCustom.Add(diffTime).Equal(timeNow))
}
```

임의로 만든 시간에 아까 구한 시간들의 차이를 더해 현재 시간과 맞는지 확인합니다.

true라고 출력됩니다.