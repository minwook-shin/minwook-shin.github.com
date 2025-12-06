---
layout: post
title: Golang 통화 서식 포맷팅 accounting 라이브러리 알아보기
---

오늘은 Golang으로 통화 포맷팅 출력하는 accounting 라이브러리를 알아보려 합니다.

## accounting 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 accounting 패키지를 설치합니다.

```
go get github.com/leekchan/accounting
```

홈의 go 폴더에 accounting 소스코드와 패키지 파일이 생성됩니다.


## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/leekchan/accounting"
)
```

fmt와 accounting을 가져옵니다.

```go
func main() {
	AccountingStruct := accounting.Accounting{Symbol: "₩"}
```

통화 기호를 지정하여 Accounting 구조체로 생성할 수 있습니다.

```go
	fmt.Println(AccountingStruct.FormatMoney(123456789))
```

지정한 통화 기호를 적용하여 값을 출력합니다.

```go
	AccountingStruct = accounting.Accounting{Symbol: "₩", Precision: 0, Thousand: ".", Decimal: ","}
```

통화 기호, 정밀도 그리고 천 단위와 소수점 구별을 위한 기호를 지정하여 Accounting 구조체로 생성할 수 있습니다.


```go
	fmt.Println(AccountingStruct.FormatMoney(123456789))
```

지정한 통화 기호, 정밀도 그리고 천 단위와 소수점 구별을 적용하여 값을 출력합니다.


```go
	AccountingStruct = accounting.Accounting{Symbol: "KRW", Format: "%s %v", FormatNegative: "%s -%v", FormatZero: "%s --"}
```

통화 기호뿐만 아니라, 출력될 때 형식을 지정할 포맷도 지정할 수 있으며 음수가 되었을 때도 포맷을 지정할 수 있습니다.

```go
	fmt.Println(AccountingStruct.FormatMoney(123456789))
	fmt.Println(AccountingStruct.FormatMoney(-1000))
	fmt.Println(AccountingStruct.FormatMoney(0))
```

설정한 포맷을 적용하여 값을 출력합니다.

0원일 때의 포맷을 %s --로 지정했으므로 금액이 나타나지 않고, KRW --로 출력됩니다.

```go
	DefaultAccounting := accounting.DefaultAccounting("KRW", 0)

	fmt.Println(DefaultAccounting.FormatMoney(123456789))
	fmt.Println(DefaultAccounting.FormatMoney(-1000))
	fmt.Println(DefaultAccounting.FormatMoney(0))
```

기본 설정으로 Accounting을 반환할 수 있습니다.

```go
	DefaultAccounting = accounting.NewAccounting("KRW", 0, ",", ".", "%s %v", "%s -%v", "%s --")

	fmt.Println(DefaultAccounting.FormatMoney(123456789))
	fmt.Println(DefaultAccounting.FormatMoney(-1000))
	fmt.Println(DefaultAccounting.FormatMoney(0))
}
```

DefaultAccounting과 같은 기본 설정이지만, 더욱 더 많은 부분을 인자로 넘길 수 있습니다.