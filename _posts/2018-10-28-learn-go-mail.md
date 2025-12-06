---
layout: post
title: Go 언어 net/mail 알아보기
---

오늘은 Go 언어 기본 패키지에 포함되있는 net/mail로 메일을 파싱해보는 실습을 해보려 합니다.

```go
package main

import (
	"fmt"
	"io/ioutil"
	"net/mail"
	"strings"
)
```

net/mail 패키지를 가져와서 메일 주소를 파싱하거나 메일 헤더를 분석할 수 있습니다.

```go
func main() {
	adress, _ := mail.ParseAddress("example <example@example.com>")
	fmt.Println(adress.Name, adress.Address)
```

example라는 이메일 주소 소유자 이름이랑 example@example.com이라는 이메일 주소로 파싱할 수 있습니다.

```go
	var addressList = "example1 <example@example.com>, example2 <example@example.com>"
	emails, _ := mail.ParseAddressList(addressList)
	for _, i := range emails {
		fmt.Println(i.Name, i.Address)
	}
```

여려 이메일 주소를 엮어놔도 AddressList로 만들어서 for range문으로 분리할 수 있습니다.

```go
	msg := `Date: 2018-10-28
From: example <from@example.com>
To: minwook-shin <to@example.com>
Subject: test

Message body text
`
```

메일 본문을 만들어둡니다.

```go
	reader := strings.NewReader(msg)
	message, _ := mail.ReadMessage(reader)
    header := message.Header
```

메일 메시지를 읽어들여서 헤더를 구분합니다.

```go
	fmt.Println("Date:", header.Get("Date"))
	fmt.Println("From:", header.Get("From"))
	fmt.Println("To:", header.Get("To"))
	fmt.Println("Subject:", header.Get("Subject"))
```

Date, From, To, Subject를 가져와서 출력합니다.

```go
	bodyText, _ := ioutil.ReadAll(message.Body)
	fmt.Printf("%s", bodyText)
}
```

메일 본문을 출력합니다.