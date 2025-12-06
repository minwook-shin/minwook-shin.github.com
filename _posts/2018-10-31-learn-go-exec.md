---
layout: post
title: Go 언어 os/exec 알아보기
---

오늘은 Go 언어 기본 패키지에 포함되있는 os/exec로 터미널 명령을 수행하는 실습을 해보려 합니다.

```go
package main

import (
	"fmt"
	"os/exec"
)
```

os/exec 패키지로 터미널 명령을 수행하여 외부 프로그램을 불러올 수 있고, fmt 패키지로는 출력할수 있습니다.

```go
func main() {
	path, _ := exec.LookPath("go")
	fmt.Println(path)
```

해당 명령이 수행되는 경로의 환경 변수를 가져오게 됩니다.

```go
	out, _ := exec.Command("date").Output()
	fmt.Println(string(out))
```

명령어의 출력을 바이트 배열로 저장할 수 있습니다.

```go
	cmd := exec.Command("sleep", "1")
	cmd.Start()
	cmd.Wait()
```

즉시 끝나지 않는 명령은 수행하고 기다릴 수 있습니다.

```go
	cmd = exec.Command("sh", "-c", "echo hello,world!")
	stdoutStderr, _ := cmd.CombinedOutput()
	fmt.Println(string(stdoutStderr))
}
```

표준 출력과 표준 오류를 같이 반환해줍니다.