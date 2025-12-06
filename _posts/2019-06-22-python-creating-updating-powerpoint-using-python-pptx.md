---
layout: post
title: Python 파워포인트 파일로 기록할 수 있는 python-pptx 라이브러리 알아보기
---

오늘은 Python으로 파워포인트 파일을 만들 수 있고, 파일 내부의 문자열을 가져올 수도 있는 python-pptx 패키지를 알아보려 합니다.

## python-pptx 설치

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
pip install python-pptx
```

pip로 python-pptx를 설치합니다.

## 제목

```python
from pptx import Presentation
```

pptx 패키지에서 Presentation을 가져옵니다.

```python
presentation = Presentation()
title_slide_layout = presentation.slide_layouts[0]
slide = presentation.slides.add_slide(title_slide_layout)
```

Presentation 객체를 만들고, 슬라이드 레이아웃을 생성합니다.

제목이 포함된 슬라이드 레이아웃을 슬라이드에 추가합니다.

해당 작업까지는 빈 제목 슬라이드가 생성된 상태입니다.

```python
title = slide.shapes.title
title.text = "Hello, World!"
```

이제 슬라이드의 제목에 문자열을 넣을 수 있습니다.

```python
presentation.save('test.pptx')
```

완성된 Presentation 객체에서 특정 경로로 파워포인트 파일을 저장할 수 있습니다.

## 슬라이드

```python
from pptx import Presentation
```

pptx 패키지에서 Presentation을 가져옵니다.

```python
presentation = Presentation()
bullet_slide_layout = presentation.slide_layouts[1]
slide = presentation.slides.add_slide(bullet_slide_layout)
```

Presentation 객체를 만들고, 슬라이드 레이아웃을 생성합니다.

머리말 슬라이드 레이아웃을 슬라이드에 추가합니다.

해당 작업까지는 빈 제목 슬라이드가 생성된 상태입니다.

```python
shapes = slide.shapes
title_shape = shapes.title
title_shape.text = 'Adding Slide'
```

해당 슬라이드의 제목을 추가합니다.

```python
body_shape = shapes.placeholders[1]
tf = body_shape.text_frame
tf.text = 'bullet slide layout'
```

텍스트 상자에 특정 문자열을 넣습니다.

```python
p = tf.add_paragraph()
p.text = 'add 1 level paragraph'
p.level = 1
```

해당 텍스트 박스에 단락을 나눌 수 있게 할 수 있습니다.

```python
presentation.save('test2.pptx')
```

완성된 Presentation 객체에서 특정 경로로 파워포인트 파일을 저장할 수 있습니다.

## 사진

```python
slide.shapes.add_picture('test.png', Inches(1), Inches(1))
presentation.save('test3.pptx')
```

사진은 add_picture를 이용하여 우측과 윗 여백을 설정하고 파일로 저장할 수 있습니다.

## 표

```python
from pptx import Presentation
from pptx.util import Inches
```

pptx 패키지에서 Presentation과 Inches를 가져옵니다.

```python
pesentation = Presentation()
title_only_slide_layout = pesentation.slide_layouts[5]
slide = pesentation.slides.add_slide(title_only_slide_layout)
```

Presentation 객체를 만들고, 슬라이드 레이아웃을 생성합니다.

제목만 존재하는 슬라이드 레이아웃을 슬라이드에 추가합니다.

해당 작업까지는 빈 제목 슬라이드가 생성된 상태입니다.

```python
shapes = slide.shapes
shapes.title.text = 'Adding Table'
```

이제 슬라이드의 제목에 문자열을 넣을 수 있습니다.

```python
table = shapes.add_table(rows=2, cols=2, left=Inches(2.0), top=Inches(2.0), width=Inches(6.0), height=Inches(1.0)).table
```

제목 아래에 표를 생성할 수 있습니다.

해당 코드는 2열 2행의 표가 만들어지며, 여백은 2인치를 줬습니다.

```python
table.cell(0, 0).text = 'name'
table.cell(0, 1).text = 'desc'

table.cell(1, 0).text = 'test_name'
table.cell(1, 1).text = 'test desc'

pesentation.save('test4.pptx')
```

표의 각자 셀에 값을 입력하고 파워포인트 파일로 저장할 수 있습니다.

## 문자열 가져오기

```python
from pptx import Presentation
```

pptx 패키지에서 Presentation을 가져옵니다.

```python
presentation = Presentation("test2.pptx")
```

특정 경로의 파워포인트 파일을 가져옵니다.

```python
text_runs = []
for slide in presentation.slides:
    for shape in slide.shapes:
        for run in shape.text_frame.paragraphs:
            text_runs.append(run.text)

print(text_runs)
```

해당 파일의 슬라이드 모든 문자열을 가져올 수 있습니다.