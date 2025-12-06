---
layout: post
title: Python 워드 파일로 기록할 수 있는 python-docx 라이브러리 알아보기
---

오늘은 Python으로 워드 파일을 만들 수 있고, 가져올 수도 있는 python-docx 패키지를 알아보려 합니다.

## python-docx 설치

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
pip install python-docx
```

pip로 python-docx를 설치합니다.

## 예제

```python
from docx import Document
```

docx 패키지에서 Document를 가져옵니다.

```python
document = Document()
```

Document 객체를 생성합니다.

```python
document.add_heading('Test Title', 0)
```

헤딩을 준 문자열을 기록합니다.

```python
document.add_heading('Heading, level 1', level=1)
```

헤딩의 레벨을 정해서 부여할 수도 있습니다.

```python
paragraph = document.add_paragraph('paragraph test')
```

새로 추가할 단락을 추가합니다.

```python
paragraph.add_run('bold').bold = True
paragraph.add_run(' and ')
paragraph.add_run('italic.').italic = True
```

볼드체, 이텔릭체를 설정할 수 있습니다.

```python
document.add_paragraph('Intense quote', style='Intense Quote')
document.add_paragraph('unordered list', style='List Bullet')
document.add_paragraph('ordered list', style='List Number')
```

스타일을 통해 글머리를 지정할 수 있습니다.

```python
document.add_picture('test.png')
```

사진을 첨부할 수 있습니다.

```python
records = (
    (1, '100', 'java'),
    (2, '101', 'c'),
    (3, '102', 'python')
)
```

튜플로 테이블에 넣을 데이터를 구성합니다.

```python
table = document.add_table(rows=1, cols=3)
cells = table.rows[0].cells
cells[0].text = 'first'
cells[1].text = 'second'
cells[2].text = 'third'
```

테이블을 생성하고 각각의 셀의 헤더에 값을 지정합니다.

```python
for i, j, k in records:
    row_cells = table.add_row().cells
    row_cells[0].text = str(i)
    row_cells[1].text = j
    row_cells[2].text = k
```

반복문을 통해서 각자의 헤더 아래에 순서대로 대입됩니다.

```python
document.add_page_break()
```

새로운 페이지로 넘어갈 수 있습니다.

```python
document.save('test.docx')
```

만든 document를 test.docx라는 파일명으로 저장합니다.

```python
f = open('test.docx', 'rb')
document = Document(f)
f.close()
```

위와 같이 이미 있는 워드 파일도 Document 객체로 불러올 수 있습니다.