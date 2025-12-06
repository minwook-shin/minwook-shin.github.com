---
layout: post
title: Python 엑셀 xlsx 파일을 만들 수 있는 XlsxWriter 라이브러리 알아보기
---

오늘은 Python으로 엑셀의 xlsx 포맷 파일을 만들 수 있는 XlsxWriter 패키지를 알아보려 합니다.

## XlsxWriter 설치

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
pip install XlsxWriter
```

pip로 XlsxWriter를 설치합니다.

## hello world!

```python
import xlsxwriter
```

xlsxwriter를 가져옵니다.

```python
workbook = xlsxwriter.Workbook('hello.xlsx')
worksheet = workbook.add_worksheet()
worksheet.write('A1', 'Hello world')
```

hello.xlsx라는 파일을 만들고 워크시트를 추가합니다.

그리고 해당 시트의 A1 위치에 Hello world를 기록합니다.

```python
workbook.close()
```

모든 작업을 끝내고 파일을 닫습니다.

## write

이번에는 tuple을 이용해서 엑셀로 기록해보려 합니다.

```python
import xlsxwriter
```

xlsxwriter를 가져옵니다.

```python
workbook = xlsxwriter.Workbook('test1.xlsx')
worksheet = workbook.add_worksheet()
```

test1.xlsx라는 파일을 만들고 워크시트를 추가합니다.

```python
test_tuple = (
    ['name', 'count'],
    ['test_name_1', 10],
    ['test_name_2', 20],
    ['test_name_3', 30],
)
```

엑셀에 기록할 튜플 타입의 변수를 만듭니다.

```python
row = 0
for name, count in test_tuple:
    worksheet.write(row, 0, name)
    worksheet.write(row, 1, count)
    row += 1
```

for문을 수행하면서 각 row에 0번째, 1번째의 칸에 값을 입력해줍니다.

```python
worksheet.write(row, 1, '=SUM(B2:B4)')
```

마지막으로 엑셀 sum 함수를 이용하여 더해줄 수도 있습니다.

```python
workbook.close()
```

모든 작업을 끝내고 파일을 닫습니다.

## chart

차트도 파이썬에서 만들 수 있습니다.

```python
import xlsxwriter
```

xlsxwriter를 가져옵니다.

```python
workbook = xlsxwriter.Workbook('test2.xlsx')
worksheet = workbook.add_worksheet()
```

test2.xlsx라는 파일을 만들고 워크시트를 추가합니다.

```python
data = [10, 20, 30, 40, 50]
worksheet.write_column('A1', data)
```

데이터를 가지고 A1위치에 작성해줍니다.

```python
chart = workbook.add_chart({'type': 'line'})
chart.add_series({'values': '=Sheet1!$A$1:$A$5'})
worksheet.insert_chart('C1', chart)
```

line 형식의 차트를 추가하고, 값을 A1부터 A5까지 지정해줍니다.

마지막으로 C1의 위치에 차트를 그려줍니다.

```python
workbook.close()
```

모든 작업을 끝내고 파일을 닫습니다.

## picture

```python
worksheet.insert_image('A1', 'test.png')
```

그림도 위와 같은 방식으로 시트를 만들고, insert_image로 특정 위치에 특정 그림을 넣을 수 있습니다.