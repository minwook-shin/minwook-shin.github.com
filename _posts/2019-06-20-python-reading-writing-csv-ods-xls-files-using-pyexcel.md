---
layout: post
title: Python 엑셀 파일이나 CSV로 내보낼 수 있는 pyexcel 라이브러리 알아보기
---

오늘은 Python으로 메모리상이나 물리적인 csv, 엑셀 파일로 저장할 수 있고, SQLAlchemy나 Django 모델로 이을 수 있는 pyexcel 패키지를 알아보려 합니다.

## pyexcel 설치

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
pip install pyexcel
```

pip로 pyexcel을 설치합니다.

```
pip install pyexcel-text
pip install pyexcel-xls
```

터미널에 테이블 형식으로 출력하거나, 엑셀 파일을 조작할 때는 위와 같이 추가로 설치해야 합니다.

## text

```python
import pyexcel
```

pyexcel을 가져옵니다.

```python
sheet = pyexcel.Sheet()
```

Sheet를 생성합니다.

```python
sheet.name = "name"
sheet.ndjson = """
{"year": ["2017", "2018", "2019", "2020", "2021"]}
{"user": [129, 253, 304, 403, 545]}
{"visit": [203, 403, 632, 832, 1023]}
""".strip()

print(sheet)
```

값을 대입하고 출력하면 아래와 같이 테이블 형식의 데이터가 출력됩니다.

```
pyexcel sheet:
+-------+------+------+------+------+------+
| year  | 2017 | 2018 | 2019 | 2020 | 2021 |
+-------+------+------+------+------+------+
| user  | 129  | 253  | 304  | 403  | 545  |
+-------+------+------+------+------+------+
| visit | 203  | 403  | 632  | 832  | 1023 |
+-------+------+------+------+------+------+
```

위와 같이 출력되려면 pyexcel-text 패키지가 필요합니다.

## excel

엑셀로도 내보내고 가져올 수 있습니다.

```python
import pyexcel
```

pyexcel 패키지를 가져옵니다.

```python
test_dict = [
    {
        "type": "PushEvent",
        "created_at": "2019-01-01T07:58:30Z",
        "id": "0000000001",
        "public": True,
    },
    {
        "type": "PushEvent",
        "created_at": "2019-01-02T07:58:30Z",
        "id": "0000000002",
        "public": True,
    },
    {
        "type": "PushEvent",
        "created_at": "2019-01-03T07:58:30Z",
        "id": "0000000003",
        "public": True,
    },
]
```

위 예시는 github event api의 json 코드를 많이 생략하고 가져왔습니다.

```python
pyexcel.save_as(records=test_dict, dest_file_name="test.xls")
```

위 딕셔너리를 넣고 test.xls라는 엑셀파일을 내보냅니다.

```python
records = pyexcel.iget_records(file_name="test.xls")
for record in records:
    print(record['created_at'], record['id'])
```

test.xls라는 엑셀파일로부터 레코드 목록을 얻어서 created_at, id를 출력합니다.


## database

데이터베이스에 연결해서 엑셀파일로 보낼 수 있습니다.

```python
engine = create_engine("sqlite:///test.db")
Base = declarative_base()
Session = sessionmaker(bind=engine)

class test_entity(Base):
    __tablename__ = 'test'
    id = Column(Integer, primary_key=True)
    name = Column(String)

Base.metadata.create_all(engine)
```

데이터베이스의 엔진과 필드 엔티티를 정의합니다.

```python
session = Session()
pyexcel.save_as(file_name="test.xls",
                name_columns_by_row=0,
                dest_session=session,
                dest_table=test_entity)
```

엑셀의 값을 넣기 원하는 세션, 테이블 객체를 정의합니다.

```python
sheet = pyexcel.get_sheet(session=session, table=test_entity)
print(sheet)
```

반대로 특정 데이터베이스 세션으로부터 시트를 가져올 수도 있습니다.