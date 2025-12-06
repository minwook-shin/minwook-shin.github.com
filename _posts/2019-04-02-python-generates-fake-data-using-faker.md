---
layout: post
title: Python Faker로 가짜 더미 데이터 만들어서 사용하기
---

오늘은 Python에서 프로그래밍할 때에 필요한 가짜 더미 데이터를 생성해주는 Faker라는 패키지를 알아보려 합니다.

## 개요

단위 테스트를 진행할 때에 필요한 특정 더미 데이터를 준비할 수 있는 패키지로서, 여러 함수들이 데이터의 덤프를 생성하게 도와줍니다.

## Github

https://github.com/joke2k/faker

## 라이선스

MIT 라이선스입니다.

## 설치

우선 virtualenv로 파이썬 환경을 분리하고

```
pip install Faker
```

pip로 Faker를 설치합니다.

## 실습

```python
from faker import Faker
```

faker 패키지를 준비합니다.

```python
fake = Faker('ko_KR')
```

Faker 객체를 만듭니다.

만약 특정 언어로 구현된 더미 데이터가 필요하다면, 인자로 국가 코드를 넣습니다.

```python
print("random_int",fake.random_int(min=0, max=999))
print("random_element",fake.random_element(elements=('a', 'b', 'c')))
print("random_elements",fake.random_elements(elements=('a', 'b', 'c'), length=None, unique=False))
print("random_choices",fake.random_choices(elements=('a', 'b', 'c'), length=None))
print("random_letter",fake.random_letter())
print("random_digit",fake.random_digit())
```

난수 더미 데이터를 생성합니다.

정수 혹은 미리 주어진 항목들로 구성할 수 있습니다.

```python
print("randomize_nb_elements",fake.randomize_nb_elements(number=10, le=False, ge=False, min=None, max=None))
print("random_letters",fake.random_letters(length=16))
print("random_number",fake.random_number(digits=None, fix_len=False))
print("random_uppercase_letter",fake.random_uppercase_letter())
print("random_lowercase_letter",fake.random_lowercase_letter())
```

숫자나 문자를 무작위로 더미 데이터를 생성합니다.

```python
print("numerify",fake.numerify(text="###"))
print("lexify",fake.lexify(text="????", letters="abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ"))
print("bothify",fake.bothify(text="## ??", letters="abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ"))
```

숫자나 주어진 문자를 형식에 맞게 더미 데이터를 생성합니다.

```python
print("random_digit_not_null_or_empty",fake.random_digit_not_null_or_empty())
print("random_digit_or_empty",fake.random_digit_or_empty())
print("random_digit_not_null",fake.random_digit_not_null())
```

조건에 따라 숫자가 나올 수 있고, null 값, 또는 빈 값이 나올 수도 있습니다.

```python
print("address",fake.address())
print("street_name",fake.street_name())
print("city",fake.city())
print("country",fake.country())
print("postcode",fake.postcode())
print("country_code",fake.country_code(representation="alpha-2"))
print("street_address",fake.street_address())
```

집 주소를 더미 데이터로 생성합니다.

```python
print("color_name",fake.color_name())
print("rgb_css_color",fake.rgb_css_color())
```

색상을 더미 데이터로 생성합니다.

```python
print("company",fake.company())
```

가상의 회사 이름을 더미 데이터로 생성합니다.

```python
print("credit_card_full",fake.credit_card_full(card_type=None))
```

가상의 신용카드 정보를 더미 데이터로 생성합니다.

```python
print("date",fake.date(pattern="%Y-%m-%d", end_datetime=None))
print("timezone",fake.timezone())
```

무작위로 날짜를 생성합니다.

```python
print("file_name",fake.file_name(category=None, extension=None))
print("file_path",fake.file_path(depth=1, category=None, extension=None))
```

무작위로 파일의 이름과 경로를 생성합니다.

```python
print("latlng",fake.latlng())
```

지도에서의 위도와 경도를 무작위로 생성합니다.

```python
print("uri_extension",fake.uri_extension())
print("ascii_safe_email",fake.ascii_safe_email())
print("uri",fake.uri())
print("ipv4",fake.ipv4(network=False, address_class=None, private=None))
```

ip와 url, 그리고 이메일 주소를 무작위로 생성합니다.

```python
print("job",fake.job())
```

가상의 직업을 무작위로 생성합니다.

```python
print("sentences",fake.sentences(nb=3, ext_word_list=None))
print("text",fake.text(max_nb_chars=200, ext_word_list=None))
print("paragraphs",fake.paragraphs(nb=3, ext_word_list=None))
print("word",fake.word(ext_word_list=None))
```

문자열, 문장, 단어, 단락을 무작위로 생성합니다.

```python
print("locale",fake.locale())
print("language_code",fake.language_code())
```

언어 코드를 무작위로 생성합니다.

```python
print("profile",fake.profile(fields=None, sex=None))
print("name_female",fake.name_female())
print("name_male",fake.name_male())
print("phone_number",fake.phone_number())
```

가상의 프로필과 이름, 핸드폰 번호를 생성합니다.

```python
print("user_agent",fake.user_agent())
print("chrome",fake.chrome(version_from=13, version_to=63, build_from=800, build_to=899))
print("firefox",fake.firefox())
print("safari",fake.safari())
print("internet_explorer",fake.internet_explorer())
```

유저 에이전트와 각 브라우저의 버전을 무작위로 생성합니다.
