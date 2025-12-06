---
layout: post
title: Python 엑셀 파일로 쓰고 읽는 openpyxl 라이브러리 알아보기
---

오늘은 Python으로 엑셀의 워크북, 시트, 셀을 조작하여 엑셀 파일을 만들어 낼 수 있는 openpyxl 패키지를 알아보려 합니다.

## openpyxl 설치

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
pip install openpyxl
```

pip로 openpyxl을 설치합니다.

## 예제

```python
from openpyxl import Workbook
```

openpyxl 패키지에서 Workbook을 가져옵니다.

```python
workbook = Workbook()
```

엑셀 파일의 여러 시트가 묶여있는 워크북에 해당되는 객체를 생성합니다.

```python
worksheet = workbook.active
worksheet.title = "New Title"
print(workbook.sheetnames)
```

특정 시트를 활성화하고 제목을 정할 수 있습니다.

```python
bonus_worksheet = workbook.create_sheet("test_sheet")
print(bonus_worksheet)
```

혹은 별도의 시트를 만들 수도 있습니다.

## 대입

```python
cell = worksheet['A4']
worksheet['A4'] = 4
print(cell)
print(cell.value)
```

A4 위체서 특정 수를 대입하여 기록할 수 있습니다.

값은 value 필드에 저장됩니다.

```python
cell2 = worksheet.cell(row=4, column=2, value=10)
print(cell2)
print(cell2.value)
```

원하는 행과 열에 특정 값을 대입할 수 있습니다.

이 역시 값은 value 필드에 저장됩니다.

## 셀

```python
col_range = bonus_worksheet['A:B']
for i in col_range:
    for j in i:
        print(j)
```

```python
row_range = bonus_worksheet[1:5]
for i in row_range:
    for j in i:
        print(j)
```

```python
cell_range = bonus_worksheet['A1':'B2']
for i in cell_range:
    for j in i:
        print(j)
```

슬라이스를 사용하여 셀 범위를 지정할 수 있습니다.

지정한 셀 범위로 반복문을 이용하면 해당 셀에 접근할 수 있습니다.

## 저장하기

```python
workbook.save('test.xlsx')
```

워크북에 등록된 워크시트의 셀에 값을 대입하고 해당 워크북을 엑셀 파일로 저장할 수 있습니다.

## 불러오기

```python
from openpyxl import load_workbook

workbook = load_workbook('test.xlsx')
print(workbook.sheetnames)
```

load_workbook으로 불러올 수 있습니다.