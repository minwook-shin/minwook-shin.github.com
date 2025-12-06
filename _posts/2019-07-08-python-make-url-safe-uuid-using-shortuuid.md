---
layout: post
title: Python UUID 생성하는 shortuuid 라이브러리 알아보기
---

오늘은 Python에서 짧고 안전한 UUID를 생성하는 shortuuid 라이브러리를 알아보려 합니다.

파이썬의 기본 uuid 모듈을 사용하여 uuid를 생성한 뒤에 base57로 변환하면서 모효한 문자들을 제거한 라이브러리입니다.

## shortuuid 설치

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
pip install shortuuid
```

pip로 shortuuid를 설치합니다.

## 기본 uuid 생성

```python
import uuid
```

우선 파이썬에 기본적으로 들어가있는 uuid 라이브러리를 먼저 살펴봅니다.

```python
host_uuid = uuid.uuid1()
print(host_uuid)

random_uuid = uuid.uuid4()
print(random_uuid)
```

호스트와 시간을 조합하여 만드는 uuid1과 난수로 만드는 uuid4가 있습니다.

```python
url = "http://www.google.com"

md5_url_uuid = uuid.uuid3(uuid.NAMESPACE_URL, url)
print(md5_url_uuid)

sha_1_url_uuid = uuid.uuid5(uuid.NAMESPACE_URL, url)
print(sha_1_url_uuid)
```

네임스페이스와 이름으로 각각 md5와 sha1으로 생성하는 uuid3, uuid5가 있습니다.

## shortuuid 예제

```python
import shortuuid
```

shortuuid를 가져옵니다.

```python
print(shortuuid.uuid())
```

간단하게 uuid를 생성할 수 있습니다.

만약 위와 같이 이름을 지정하지 않으면 난수로 만드는 uuid4와 같습니다.

```python
print(shortuuid.uuid(name="example"))
print(shortuuid.uuid(name="http://www.google.com"))
```

이름이 존재하면 uuid5로 같으며, 만약 문자열에 http가 존재하면 NAMESPACE_URL로 설정됩니다.

```python
print(shortuuid.ShortUUID().random(length=50))
```

os.urandom으로 임의의 문자열을 생성할 수 있습니다.

```python
print(shortuuid.get_alphabet())
```

새 UUID를 생성하는 데 사용중인 알파벳을 볼 수 있습니다.

이 라이브러리를 기본적으로 0, 1, l, I, O를 사용하지 않는 것을 확인할 수 있습니다.

만약 재지정하려면 set_alphabet을 사용합니다.

```python
import uuid

s = shortuuid.encode(uuid.uuid4())
print(s)
```

기존 uuid 패키지로 생성한 것도 encode하고 decode할 수 있습니다.

```python
su = shortuuid.ShortUUID()
print(su.uuid())
```

객체로 사용할 수도 있습니다.