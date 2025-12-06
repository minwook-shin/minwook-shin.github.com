---
layout: post
title: Python SQL 구문을 파싱하는 sqlparse 라이브러리 알아보기
---

오늘은 Python에서 SQL 구문을 파싱할 수 있는 sqlparse 라이브러리를 알아보려 합니다.

## python-sqlparse 설치

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
pip install sqlparse
```

pip로 sqlparse 를 설치합니다.

## 예제

```python
import sqlparse
```

sqlparse를 가져옵니다.

```python
sql = 'select * from db; select * from db_slave;'
print(sqlparse.split(sql))
```

sql 구문을 작성하고 split으로 나눌 수 있습니다.

아래와 같이 서로 분리됩니다.
['select * from db;', 'select * from db_slave;']

```python
sql = 'select * from db where id in (select id from db);'
print(sqlparse.format(sql, reindent=False, keyword_case='lower'))
sql = 'select * from db where id in (select id from db);'
print(sqlparse.format(sql, reindent=False, keyword_case='upper'))
```

주어진 옵션에 따라서 문자열을 포맷팅 합니다.

키워드를 소문자로 할 지, 대문자로 할 지에 대한 옵션에 따라 아래와 같이 출력됩니다.

select * from db where id in (select id from db);
SELECT * FROM db WHERE id IN (SELECT id FROM db);

```python
sql = 'select * from db where id in (select id from db);'
print(sqlparse.format(sql, reindent=True, keyword_case='lower'))
sql = 'select * from db where id in (select id from db);'
print(sqlparse.format(sql, reindent=True, keyword_case='upper'))
```

주어진 옵션에 따라서 문자열을 포맷팅 합니다.

reindent를 참으로 설정하면 아래와 같이 인덴트를 재설정합니다.

select *
from db
where id in
    (select id
     from db);
SELECT *
FROM db
WHERE id IN
    (SELECT id
     FROM db);

```python
sql = 'select * from db where id in (select id from db);'
print(sqlparse.format(sql, reindent=True, identifier_case='lower'))
sql = 'select * from db where id in (select id from db);'
print(sqlparse.format(sql, reindent=True, identifier_case='upper'))
```

주어진 옵션에 따라서 문자열을 포맷팅 합니다.

이번에는 키워드가 아닌 식별자를 대소문자로 변환할 수 있습니다.

아래와 같이 출력됩니다.

select *
from db
where id in
    (select id
     from db);
select *
from DB
where ID in
    (select ID
     from DB);

```python
sql = 'select * from db where id in (select id from db);'
print(sqlparse.format(sql, reindent=True, identifier_case='lower',keyword_case='lower'))
sql = 'select * from db where id in (select id from db);'
print(sqlparse.format(sql, reindent=True, identifier_case='upper',keyword_case='upper'))
```

키워드와 식별자를 모두 대소문자로 변환할 수 있습니다.

select *
from db
where id in
    (select id
     from db);
SELECT *
FROM DB
WHERE ID IN
    (SELECT ID
     FROM DB);

```python
sql = 'delete from db where id = 0'
parsed = sqlparse.parse(sql)
print(parsed)
for i in parsed:
    print(i)
```

파싱한 sql 구문을 반복하여 출력할 수 있습니다.