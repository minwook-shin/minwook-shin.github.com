---
layout: post
title: Go 언어 유닛테스트 대하여 배워보기 
---

오늘은 Go 언어에서 유닛테스트를 해보는 방법을 작성하면서 배워보려 합니다.

```go
package main

import (
	"fmt"
)

func main() {
	fmt.Println(CalcUnitTest(5,10,15))
}

// CalcUnitTest function
func CalcUnitTest(n ...int)int{
	i := 0
	for _,j := range n{
		i += j
	}
	return i
}
```

우선 평소와 같이 메인 함수를 작성해줍니다.

테스트할 함수를 따로 작성하기 위해 가변인자를 가지는 함수를 만들어줍니다.

이 함수는 가변인자로 받는 모든 정수형 변수를 전부 더해서 반환하는 함수입니다.

출력하면 5와 10과 15를 더해서 30이 출력됩니다.

```go
package main_test

import (
    "unitTest"
    "testing"
)
 
func TestSum(t *testing.T) {
    a := main.CalcUnitTest(5, 10, 15)
 
	if a != 30 {
		t.Error()
	}
}
```

이 작업을 메인 함수에서 테스트하는 것보다 유닛테스트로 여러 테스트 케이스를 구동할 수 있습니다.

파일을 따로 만들고 위와 같이 작성해봅니다.

패키지 이름은 ```[테스트할 패키지 이름]_test```로 해야합니다.

그리고나서 testing 패키지를 import해줍니다.

테스트 케이스 함수의 이름은 Test라고 시작하며 인자는 *testing.T 타입의 포인터를 받고, 
테스트할 때 오류를 나타내기 위해 Error 함수를 사용해줍니다.

만약 Error가 발생하지 않는다면 Success: Tests passed.라고 출력되며 아니면 어느 부분에서 오류가 발생했는지 표기해줍니다.