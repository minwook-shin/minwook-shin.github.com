---
layout: post
title: Golang 무작위 더미 데이터 생성 gofakeit 라이브러리 알아보기
---

오늘은 Golang으로 무작위 더미 데이터를 생성할 수 있는 gofakeit 라이브러리를 알아보려 합니다.

## gofakeit 설치

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

go get으로 gofakeit 패키지를 설치합니다.

```
go get github.com/brianvoe/gofakeit
```

홈의 go 폴더에 gofakeit 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/brianvoe/gofakeit"
)
```

fmt와 gofakeit을 가져옵니다.

```go
func main() {
	fmt.Println("Person : ", gofakeit.Person())
	fmt.Println("Name : ", gofakeit.Name())
	fmt.Println("NamePrefix : ", gofakeit.NamePrefix())
	fmt.Println("NameSuffix : ", gofakeit.NameSuffix())
	fmt.Println("FirstName : ", gofakeit.FirstName())
	fmt.Println("LastName : ", gofakeit.LastName())
	fmt.Println("Gender : ", gofakeit.Gender())
```

인적사항에 대한 더미 정보를 반환해서 출력합니다.

```go
	fmt.Println("Contact : ", gofakeit.Contact())
	fmt.Println("Email : ", gofakeit.Email())
```

연락처 정보에 대한 더미 정보를 반환해서 출력합니다.

```go
	fmt.Println("Phone : ", gofakeit.Phone())
	fmt.Println("PhoneFormatted : ", gofakeit.PhoneFormatted())
```

핸드폰 번호에 대한 더미 정보를 반환해서 출력합니다.

```go
	fmt.Println("Username : ", gofakeit.Username())
	fmt.Println("Password : ", gofakeit.Password(true, true, true, true, false, 16))
```

사용자 이름과 비밀번호에 대한 더미 정보를 반환해서 출력합니다.

```go
	fmt.Println("Address : ", gofakeit.Address())
	fmt.Println("City : ", gofakeit.City())
	fmt.Println("Country : ", gofakeit.Country())
	fmt.Println("CountryAbr : ", gofakeit.CountryAbr())
	fmt.Println("State : ", gofakeit.State())
	fmt.Println("StateAbr : ", gofakeit.StateAbr())
	fmt.Println("StatusCode : ", gofakeit.StatusCode())
	fmt.Println("Street : ", gofakeit.Street())
	fmt.Println("StreetName : ", gofakeit.StreetName())
	fmt.Println("StreetNumber : ", gofakeit.StreetNumber())
	fmt.Println("StreetPrefix : ", gofakeit.StreetPrefix())
	fmt.Println("StreetSuffix : ", gofakeit.StreetSuffix())
	fmt.Println("Zip : ", gofakeit.Zip())
```

집 주소에 대한 더미 정보를 반환해서 출력합니다.

```go
	fmt.Println("Latitude : ", gofakeit.Latitude())
	fmt.Println("Longitude : ", gofakeit.Longitude())
```

위도 경도에 대한 더미 정보를 반환해서 출력합니다.

```go
	fmt.Println("UUID : ", gofakeit.UUID())
```

uuid에 대한 더미 정보를 반환해서 출력합니다.

```go
	fmt.Println("Color : ", gofakeit.Color())
	fmt.Println("HexColor : ", gofakeit.HexColor())
	fmt.Println("RGBColor : ", gofakeit.RGBColor())
	fmt.Println("SafeColor : ", gofakeit.SafeColor())
```

색상에 대한 더미 정보를 반환해서 출력합니다.

```go
	fmt.Println("URL : ", gofakeit.URL())
	fmt.Println("ImageURL : ", gofakeit.ImageURL(320, 320))
	fmt.Println("DomainName : ", gofakeit.DomainName())
	fmt.Println("DomainSuffix : ", gofakeit.DomainSuffix())
	fmt.Println("IPv4Address : ", gofakeit.IPv4Address())
	fmt.Println("IPv6Address : ", gofakeit.IPv6Address())
```

URL과 도메인에 대한 더미 정보를 반환해서 출력합니다.

```go
	fmt.Println("SimpleStatusCode : ", gofakeit.SimpleStatusCode())
	fmt.Println("HTTPMethod : ", gofakeit.HTTPMethod())
	fmt.Println("UserAgent : ", gofakeit.UserAgent())
	fmt.Println("ChromeUserAgent : ", gofakeit.ChromeUserAgent())
	fmt.Println("FirefoxUserAgent : ", gofakeit.FirefoxUserAgent())
	fmt.Println("SafariUserAgent : ", gofakeit.SafariUserAgent())
```

UserAgent에 대한 더미 정보를 반환해서 출력합니다.

```go
	fmt.Println("Date : ", gofakeit.Date())
	fmt.Println("NanoSecond : ", gofakeit.NanoSecond())
	fmt.Println("Second : ", gofakeit.Second())
	fmt.Println("Minute : ", gofakeit.Minute())
	fmt.Println("Hour : ", gofakeit.Hour())
	fmt.Println("Month : ", gofakeit.Month())
	fmt.Println("Day : ", gofakeit.Day())
	fmt.Println("WeekDay : ", gofakeit.WeekDay())
	fmt.Println("Year : ", gofakeit.Year())
```

시간과 날짜에 대한 더미 정보를 반환해서 출력합니다.

```go
	fmt.Println("TimeZone : ", gofakeit.TimeZone())
	fmt.Println("TimeZoneAbv : ", gofakeit.TimeZoneAbv())
	fmt.Println("TimeZoneFull : ", gofakeit.TimeZoneFull())
	fmt.Println("TimeZoneOffset : ", gofakeit.TimeZoneOffset())
```

타임존에 대한 더미 정보를 반환해서 출력합니다.

```go
	fmt.Println("Price : ", gofakeit.Price(1000, 10000))
	fmt.Println("Currency : ", gofakeit.Currency())
	fmt.Println("CurrencyLong : ", gofakeit.CurrencyLong())
	fmt.Println("CurrencyShort : ", gofakeit.CurrencyShort())
```

통화에 대한 더미 정보를 반환해서 출력합니다.

```go
	fmt.Println("Language : ", gofakeit.Language())
	fmt.Println("LanguageAbbreviation : ", gofakeit.LanguageAbbreviation())
	fmt.Println("ProgrammingLanguage : ", gofakeit.ProgrammingLanguage())
```

언어에 대한 더미 정보를 반환해서 출력합니다.

```go
	fmt.Println("Extension : ", gofakeit.Extension())
	fmt.Println("MimeType : ", gofakeit.MimeType())
}
```

확장자에 대한 더미 정보를 반환해서 출력합니다.