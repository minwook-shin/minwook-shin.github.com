---
layout: post
title: Golang 정규표현식 모음 CommonRegex 라이브러리 알아보기
---

오늘은 Golang으로 정규표현식의 모음집인 CommonRegex 라이브러리를 알아보려 합니다.

## CommonRegex 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

맥에서 Golang의 버전을 올리려면 brew를 이용합니다.

```
brew upgrade go
```

우분투에서도 Golang의 버전을 올리려면 backports 저장소를 등록하고 apt를 이용합니다.

```
sudo add-apt-repository ppa:longsleep/golang-backports
sudo apt-get update
sudo apt-get install golang-go
```

go get으로 CommonRegex 패키지를 설치합니다.

```
go get github.com/mingrammer/commonregex
```

홈의 go 폴더에 CommonRegex 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/mingrammer/commonregex"
)
```

fmt와 commonregex를 가져옵니다.

```go
func main() {
	textString := `Jan 9th 2012 5:00PM 4:00 $9,000 test@gmail.com www.example.com 127.0.0.1 8A-99-D9-96-89-5B https://github.com/user/repo.git (519)-236-2723x341 #ffffff`
```

여러 정보들이 포함된 문자열이 담긴 변수를 만듭니다.

```go
	date := commonregex.Date(textString)
	fmt.Println(date)

	time := commonregex.Time(textString)
	fmt.Println(time)

	price := commonregex.Prices(textString)
	fmt.Println(price)

	email := commonregex.Emails(textString)
	fmt.Println(email)

	link := commonregex.Links(textString)
	fmt.Println(link)

	git := commonregex.GitRepos(textString)
	fmt.Println(git)

	phone := commonregex.PhonesWithExts(textString)
	fmt.Println(phone)

	ip := commonregex.IPs(textString)
	fmt.Println(ip)

	mac := commonregex.MACAddresses(textString)
	fmt.Println(mac)
}
```

날짜, 시간, 가격, 이메일, 링크, 깃 주소, ip 등의 정보들을 가려낼 수 있는 정규표현식 패턴으로 각자의 정보들을 추출할 수 있습니다.

해당 함수들은 regexp의 MustCompile로 분석해둔 일정 패턴으로 일치되는 문자열을 반환합니다.