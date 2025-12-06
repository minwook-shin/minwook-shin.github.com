---
layout: post
title: Python 전화번호 파싱하는 phonenumbers 라이브러리 알아보기
---

오늘은 Python에서 Google의 libphonenumber 라이브러리를 포팅한 전화번호 파싱할 수 있는 phonenumbers 라이브러리를 알아보려 합니다.

## phonenumbers 설치

우선 virtualenv로 파이썬 환경을 분리해줍니다.

```
pip3 install virtualenv
```

```
virtualenv -mvenv env
```

env라는 이름의 가상 환경을 생성합니다.

```
source env/bin/activate
```

가상환경을 폴더에서 활성화합니다.

```
pip3 install --upgrade pip
```

pip의 업그레이드가 존재하는지 확인하고 진행합니다.

```
pip install phonenumbers
```

pip로 phonenumbers를 설치합니다.

## 기본 예제

```python
import phonenumbers
```

phonenumbers를 가져옵니다.

```python
phone1 = phonenumbers.parse("+821000000000", None)
print(phone1)
type(phone1)

phone2 = phonenumbers.parse("010 0000 0000", "KR")
print(phone2)

print(phone1 == phone2)

print(phonenumbers.is_valid_number(phone1))
print(phonenumbers.is_possible_number(phone1))
```

핸드폰 번호를 파싱해서 국가 코드와 전화번호를 출력할 수 있고, 올바른 전화번호 양식인지도 확인할 수 있습니다.

```python
try:
  x = phonenumbers.parse("01000000000", None)
except phonenumbers.phonenumberutil.NumberParseException as e:
  print(e)

try:
  x = phonenumbers.parse("hello,world", None)
except phonenumbers.phonenumberutil.NumberParseException as e:
  print(e)
```

지역을 확인할 수 없거나, 핸드폰 번호가 아닐 시에 오류가 발생합니다.

```python
print(phonenumbers.format_number(phone1, phonenumbers.PhoneNumberFormat.NATIONAL))
print(phonenumbers.format_number(phone1, phonenumbers.PhoneNumberFormat.INTERNATIONAL))
print(phonenumbers.format_number(phone1, phonenumbers.PhoneNumberFormat.E164))
```

번호를 출력할 포맷을 지정할 수 있습니다.

```python
formatter = phonenumbers.AsYouTypeFormatter("KR")
print(formatter.input_digit("1"))
print(formatter.input_digit("0"))
print(formatter.input_digit("0"))
print(formatter.input_digit("0"))
print(formatter.input_digit("0"))
print(formatter.input_digit("0"))
print(formatter.input_digit("0"))
print(formatter.input_digit("0"))
```

한 문자씩 누적으로 출력할 수 있습니다.

```python
text = "Call me at 010-0000-0000 after 10 am."
for match in phonenumbers.PhoneNumberMatcher(text, "KR"):
  print(match)

for match in phonenumbers.PhoneNumberMatcher(text, "KR"):
  print(phonenumbers.format_number(match.number, phonenumbers.PhoneNumberFormat.E164))
```

문자열에서 전화번호만 추출할 수 있습니다.

## geocoder

```python
from phonenumbers import geocoder
```

phonenumbers에서 geocoder를 가져옵니다.

```python
kr_number = phonenumbers.parse("01000000000", "KR")
print(geocoder.description_for_number(kr_number, "ko"))
```

어느 나라의 전화번호인지 정해진 언어로 출력할 수 있습니다.

## timezome

```python
from phonenumbers import timezone
```

timezone을 가져옵니다.

```python
kr_number = phonenumbers.parse("01000000000", "KR")
print(timezone.time_zones_for_number(kr_number))
```

해당 전화번호의 타임존을 알 수 있습니다.