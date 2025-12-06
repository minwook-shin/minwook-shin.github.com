---
layout: post
title: Python 네이티브 객체 데이터베이스 ZODB 라이브러리 알아보기
---

오늘은 Python을 가지고 객체로 만들 수 있는 데이터베이스 ZODB 패키지에 대하여 알아보려 합니다.

데이터베이스 트랜젝션이 가능합니다.

## ZODB 설치

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
pip install ZODB
```

pip로 ZODB를 설치합니다.

## ZODB 기본 사용법

```python
import persistent
import ZODB.FileStorage
import transaction
```

persistent 객체를 생성할 때에 필요한 패키지와 파일, 트랜젝션에 관련있는 FileStorage, transaction도 가져옵니다.

```python
class TestScore(persistent.Persistent):

    def __init__(self):
        self.score = 0

    def visit(self, i):
        self.score += i
```

persistent 객체를 만들기 위한 class를 작성해줍니다.

score라는 필드가 존재하고, visit 메소드를 통해 원하는 값을 증가시킵니다.

```python
if __name__ == '__main__':
    storage = ZODB.FileStorage.FileStorage('data.fs')
```

data.fs 확장자의 파일 스토리지를 만듭니다.

```python
    db = ZODB.DB(storage)
```

객체 데이터베이스를 만듭니다.

```python
    connection = db.open()
```

데이터베이스 연결을 진행합니다.

```python
    root = connection.root
    root.storage = TestScore()
```

루트는 데이터베이스의 최상위 수준의 네임스페이스 역활을 하며, 해당 코드에서는 persistent 객체를 넣어서 사용했습니다.

```python
    root.storage.visit(1)
    transaction.commit()
    print(root.storage.score)
```

트랜섹션 커밋하여 영구적으로 저장할 수 있습니다.

```python
    root.storage.visit(1)
    transaction.abort()
    print(root.storage.score)
```

커밋을 하고 싶지 않을 때에는 abort할 수 있습니다.

```python
    save = transaction.savepoint()
    root.storage.visit(1)
    root.storage.visit(1)
    root.storage.visit(1)
    print(root.storage.score)
    save.rollback()
    print(root.storage.score)
```

세이브 지점을 만들어서 변경점을 다시 롤백할 수 있습니다.

```python
    root.storage.visit(1)
    transaction.doom()
    print(root.storage.score)

    # root.storage.visit(1)
    # transaction.commit()
    # print(root.storage.score)
```

doom()을 하면 값을 더 이상 변경할 수 없습니다.
