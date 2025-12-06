---
layout: post
title: Golang 통화 관리 GoMoney 라이브러리 알아보기
---

오늘은 Golang으로 통화 단위를 지정하여 금전을 다룰 때에 사용할 GoMoney 라이브러리를 알아보려 합니다.

## GoMoney 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

go get으로 GoMoney 패키지를 설치합니다.

```
go get github.com/Rhymond/go-money
```

홈의 go 폴더에 GoMoney 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/Rhymond/go-money"
)
```

fmt와 go-money를 가져옵니다.

```go
func main() {
	won := money.New(100, "KRW")
```

원화를 기준으로 하는 money 객체를 만듭니다.

```go
	doubleWon, err := won.Add(won)
	if err != nil {
		panic(err)
	}
```

100원에 100원을 더 하여 200원의 money 객체를 만듭니다.

```go
	fmt.Println(doubleWon.Amount())
```

int64로 출력할 수 있습니다.

```go
	fmt.Println(doubleWon.Currency().Code)
	fmt.Println(doubleWon.Currency().Formatter())
	fmt.Println(doubleWon.Currency().Grapheme)
	fmt.Println(doubleWon.Currency().Template)
	fmt.Println(doubleWon.Currency().Decimal)
	fmt.Println(doubleWon.Currency().Thousand)
```

통화 기호부터 천 단위, 소수점 구분자까지 출력할 수 있습니다.

```go
	fmt.Println(doubleWon.Display())
	fmt.Println(doubleWon.IsPositive())
```

통화 단위를 적용한 금액을 출력합니다.

IsPositive로 양수인지 확인할 수 있습니다.

```go
	fmt.Println(doubleWon.Negative().Display())
	fmt.Println(doubleWon.Negative().IsNegative())
```

음수로 만들어서 통화 단위를 적용한 금액을 출력합니다.

IsNegative로 음수인지 확인할 수 있습니다.

```go
	zero := money.New(0, "KRW")
	fmt.Println(zero.IsZero())
	fmt.Println(doubleWon.SameCurrency(zero))
```

IsZero로 0원인지 확인할 수 있으며, 기존에 만든 money 객체와 통화 기호가 같은 지 확인할 수도 있습니다.

```go
	eq, err := doubleWon.Equals(doubleWon)
	if err != nil {
		panic(err)
	}
	fmt.Println(eq)
```

같은 금액인지도 확인할 수 있습니다.

```go
	gr, err := doubleWon.GreaterThan(won)
	if err != nil {
		panic(err)
	}
	fmt.Println(gr)

	less, err := won.LessThan(doubleWon)
	if err != nil {
		panic(err)
	}
	fmt.Println(less)
```

금액이 크거나 작은지 확인할 수 있습니다.

```go
	sub, err := doubleWon.Subtract(won)
	if err != nil {
		panic(err)
	}
	fmt.Println(sub.Display())
```

Add와 대비되는 메소드로서 금액을 차감합니다.

```go
	splitWon, err := doubleWon.Split(2)
	if err != nil {
		panic(err)
	}
```

해당 금액을 두 번으로 나눌 때에 위와 같이 사용할 수 있습니다.

```go
	a := splitWon[0].Display()
	b := splitWon[1].Display()
	fmt.Println(a, b)
}
```

200원을 두 번 나누었으므로 각자 100원으로 출력됩니다.