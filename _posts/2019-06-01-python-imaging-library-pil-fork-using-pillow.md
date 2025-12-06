---
layout: post
title: Python 이미지 처리하는 Pillow 라이브러리 알아보기
---

오늘은 Python으로 PIL을 대체하여 이미지 처리할 수 있는 라이브러리인 Pillow를 적용해보려 합니다.

## Pillow 설치

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
pip install Pillow
```

pip로 Pillow를 설치합니다.

## convert

```python
import os
from PIL import Image

input_img = input()
f, e = os.path.splitext(input_img)

try:
    Image.open(input_img).save("original.jpeg")
except IOError:
    pass
```

다른 확장자를 jpeg 사진으로 저장할 수 있습니다.

## thumnail

```python
from PIL import Image

try:
    im = Image.open("original.png")
    im.thumbnail((128, 128))
    im.save("original.thumbnail", "JPEG")
except IOError:
    pass
```

썸네일을 만들 수 있습니다.

## identity

```python
from PIL import Image

try:
    with Image.open("original.png") as im:
        print(im.format, im.size, im.mode)
except IOError:
    pass
```

이미지 파일의 정보를 식별할 수 있습니다.

## crop

```python
from PIL import Image

try:
    with Image.open("original.png") as im:
        box = (100, 100, 400, 400)
        region = im.crop(box)
        region = region.transpose(Image.ROTATE_180)
        im.paste(region, box)
        im.save("pasted_img.png", "PNG")
        print(region)
except IOError:
    pass
```

이미지를 자르고 붙일 수 있습니다.

## roll

```python
from PIL import Image

try:
    with Image.open("original.png") as im:
        x_size, y_size = im.size

        delta = 300 % x_size

        part1 = im.crop((0, 0, delta, y_size))
        part2 = im.crop((delta, 0, x_size, y_size))
        im.paste(part1, (x_size - delta, 0, x_size, y_size))
        im.paste(part2, (0, 0, x_size - delta, y_size))
        im.save("roll_img.png", "PNG")
except IOError:
    pass
```

이미지를 말린 모양으로 변환할 수 있습니다.

## geometrical

```python
from PIL import Image

try:
    with Image.open("original.png") as im:
        out = im.resize((128, 128))
        out.save("resize_img.png", "PNG")
        out = im.rotate(45)  # degrees counter-clockwise
        out.save("rotate_img.png", "PNG")
except IOError:
    pass
```

지오메트리 변환을 통해 이미지의 크기를 변경하거나 회전시킬 수 있습니다.

## filter

```python
from PIL import Image
from PIL import ImageFilter


try:
    with Image.open("original.png") as im:
        out = im.filter(ImageFilter.BoxBlur(20))
        out.save("filter_img.png", "PNG")
except IOError:
    pass
```

별도로 ImageFilter를 가져와서 블러 효과 처리를 할 수 있습니다.
