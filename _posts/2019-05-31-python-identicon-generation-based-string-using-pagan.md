---
layout: post
title: Python 문자열 해싱하여 아바타 사진 생성하는 pagan 라이브러리 알아보기
---

오늘은 Python으로 입력 문자열을 해시하여 프로필 사진으로 사용할 고유 아바타(identicons) 이미지를 생성할 수 있는 라이브러리인 pagan을 적용해보려 합니다.

## pagan 설치

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
pip install keyboard
```

pip로 keyboard를 설치합니다.

## cli

```bash
>> pagan (문자열)
```

cli 프로그램으로도 사용할 수 있습니다.

- \-h

- \-\-show

- \-\-output OUTPUT

- \-\-hash HASH

추가적으로 보여줄 설정과 암호화할 수준을 정해둘 수도 있습니다.

## 아바타 생성

파이썬 코드로도 생성하고 출력할 수 있습니다.

```python
import pagan
```

pagan 라이브러리를 가져옵니다.

```python
input_name = 'blog'

img = pagan.Avatar(input_name, pagan.SHA512)
```

암호화하여 프로필 사진으로 만들 문자열을 지정합니다.

```
MD5 = pagan.generator.HASH_MD5
SHA1 = pagan.generator.HASH_SHA1
SHA224 = pagan.generator.HASH_SHA224
SHA256 = pagan.generator.HASH_SHA256
SHA384 = pagan.generator.HASH_SHA384
SHA512 = pagan.generator.HASH_SHA512
```

기본적으로 암호화는 md5로 수행되지만, SHA1, SHA224, SHA256, SHA384,SHA512로도 할 수 있습니다.

내부적인 코드로 보면 hashlib를 사용합니다.

```python
img.show()
```

컴퓨터에 설치되어 있는 이미지 뷰어 프로그램으로 프로필 사진을 보여주게 합니다.

```python
out_path = 'output/'
filename = input_name

img.save(out_path, filename)
```

최종적으로 output 폴더에 입력한 문자열이 파일 이름으로 저장됩니다.
