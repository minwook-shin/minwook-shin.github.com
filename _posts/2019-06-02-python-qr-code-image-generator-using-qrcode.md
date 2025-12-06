---
layout: post
title: Python QR 코드를 생성하는 qrcode 라이브러리 알아보기
---

오늘은 Python으로 QR 코드를 생성하는 라이브러리인 qrcode를 사용해보려 합니다.

## qrcode 설치

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
pip install qrcode
```

pip로 qrcode를 설치합니다.

```
pip install Pillow
```

단, 이미지를 생성하기 위해서는 PIL의 역할이 필요합니다.

Pillow도 설치합니다.

## hello, world!

```python
import qrcode
```

qrcode를 가져옵니다.

```python
img = qrcode.make('hello,world!')
img.save("hello.jpg")
```

기본적으로 qrcode는 위와 같이 간단하게 문자열과 생성할 파일명을 작성합니다.

make로 원하는 문자열을 넣고, save로 저장된 이미지 변수를 파일로 저장합니다.

## advanced

```python
import qrcode
```

qrcode를 가져옵니다.

```python
qr = qrcode.QRCode(
    version=1,
    error_correction=qrcode.constants.ERROR_CORRECT_L,
    box_size=10,
    border=4,
)
```

QRCode 객체를 만들면서 상세하게 제어할 수 있습니다.

```python
qr.add_data('hello,world!')
```

qr 코드의 데이터를 추가합니다.

```python
img = qr.make_image(fill_color="white", back_color="black")
img.save("hello2.jpg")
```

fill_color 속성으로 원하는 색상으로 채우거나, back_color 속성으로 원하는 배경으로 채울 수 있습니다.

## svg

```python
import qrcode.image.svg
```

qrcode에서 svg도 만들 수 있습니다.

```python
factory = qrcode.image.svg.SvgImage
img = qrcode.make('hello,world!', image_factory=factory)
img.save("first.svg")
```

SVG 이미지 빌더인 qrcode.image.svg.SvgImage를 image_factory 속성에 넣어서 svg 파일로 생성합니다.
