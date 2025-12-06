---
layout: post
title: Python 테이블 형식의 데이터 집합을 출력하는 Tablib 라이브러리 알아보기
---

오늘은 Python으로 테이블 형식의 데이터들의 집합을 엑셀, csv, json, yaml 등으로 내보내게 해주는 Tablib를 알아보려 합니다.

## Tablib 설치

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
pip install tablib
```

pip로 Tablib를 설치합니다.

## 데이터 집합 다루기

```python
import tablib

headers = ('id', 'name')

data = [
    ('test_id_1', 'name_1'),
    ('test_id_2', 'name_2')
]
```

헤더와 데이터를 준비합니다.

```python
data = tablib.Dataset(*data, headers=headers)
print(data)
print(type(data))

data.append(('test_id_3', 'name_3'))
data.append_col((1, 2, 3), header='count')

print(data)
print(data['id'])
```

```
id       |name  |count
---------|------|-----
test_id_1|name_1|1    
test_id_2|name_2|2    
test_id_3|name_3|3    
```

위와 같이 테이블 형식으로 출력되며, append로 새로운 행, 열을 추가할 수 있습니다.

## json

```python
import tablib

headers = ('id', 'name')

data = [
    ('test_id_1', 'name_1'),
    ('test_id_2', 'name_2'),
    ('test_id_3', 'name_3')
]

data = tablib.Dataset(*data, headers=headers)
print(data.export('json'))
```

json 형식으로 내보낼 수 있습니다.

## yaml

```python
import tablib

headers = ('id', 'name')

data = [
    ('test_id_1', 'name_1'),
    ('test_id_2', 'name_2'),
    ('test_id_3', 'name_3')
]

data = tablib.Dataset(*data, headers=headers)
print(data.export('yaml'))
```

yaml 형식으로 내보낼 수 있습니다.

## csv

```python
import tablib

headers = ('id', 'name')

data = [
    ('test_id_1', 'name_1'),
    ('test_id_2', 'name_2'),
    ('test_id_3', 'name_3')
]

data = tablib.Dataset(*data, headers=headers)
print(data.export('csv'))
```

csv 형식으로 내보낼 수 있습니다.

## pandas

```python
import tablib

headers = ('id', 'name')

data = [
    ('test_id_1', 'name_1'),
    ('test_id_2', 'name_2'),
    ('test_id_3', 'name_3')
]

data = tablib.Dataset(*data, headers=headers)
print(data.export('df'))
```

판다스의 데이터프레임 형식으로 내보낼 수 있습니다.

단, pandas 패키지가 설치되어 있어야 합니다.

## excel

```python
import tablib

headers = ('id', 'name')

data = [
    ('test_id_1', 'name_1'),
    ('test_id_2', 'name_2'),
    ('test_id_3', 'name_3')
]

data = tablib.Dataset(*data, headers=headers)
with open('test.xls', 'wb') as f:
    f.write(data.export('xls'))
```

엑셀 파일로 내보낼 수 있습니다.