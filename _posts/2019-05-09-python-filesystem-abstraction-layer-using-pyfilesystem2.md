---
layout: post
title: Python 파일과 디렉토리 작업하는 PyFilesystem2 라이브러리 알아보기
---

오늘은 Python에서 파일과 디렉토리 작업을 추상화시킬 수 있는 라이브러리인 PyFilesystem2 패키지에 대하여 간단히 알아보려 합니다.

## PyFilesystem2 설치

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
pip install fs
```

pip로 PyFilesystem2를 설치합니다.

## 기본 예제

```python
from fs.osfs import OSFS
```

PyFilesystem2에서 OSFS를 가져옵니다.

```python
home_fs = OSFS("~/")
print(home_fs)

dir_list = home_fs.listdir('/')
print(dir_list)
```

OSFS 객체로 홈 폴더를 가져올 수 있고, 해당 폴더의 내용도 가져올 수 있습니다.

```python
from fs import open_fs
```

PyFilesystem2에서 open_fs를 가져옵니다.

```python
home_fs = open_fs('osfs://~/')
print(home_fs.listdir('/'))
```

url로 폴더를 열 수 있습니다.

```python
my_fs = open_fs('.')
print(my_fs.tree())
```

트리 형식으로 출력할 수 있습니다.

```python
current_fs = open_fs('.')
current_fs.writetext('test.txt', 'hello world!')
current_fs.close()
```

문자열로 기록한 파일을 만들 수 있습니다.

```python
with open_fs('.') as current_fs:
    current_fs.writetext('test.txt', 'hello world!')


with open_fs('.') as current_fs:
    with current_fs.open('test.txt') as test_file:
        print(test_file.read())
```

with as로 \_\_enter\_\_ 과 \_\_exit\_\_ 를 자동으로 호출하여 close()를 별도로 하지 않아도 됩니다.

```python
current_fs = open_fs('.')
print(current_fs.listdir('/.idea'))
```

현재 폴더에서 하위 폴더의 목록을 출력할 수 있습니다.

```python
current_fs = open_fs('.')
directory = list(current_fs.scandir('/.idea'))
for i in directory:
    print(i)
```

현재 폴더에서 하위 폴더의 목록을 iterator하게 할 수 있습니다.

```python
current_fs = open_fs('.')
if not current_fs.exists('text'):
    folder_fs = current_fs.makedirs('text')
    folder_fs.touch('__init__.py')
    folder_fs.writetext('README.md', "# hello,world!")
    print(folder_fs.listdir('/'))
```

폴더를 만들고, 파일을 생성하거나 문자열을 기록할 수 있습니다.

```python
current_fs = open_fs('.')
for path in current_fs.walk.files(filter=['*.py']):
    print(path)
```

원하는 폴더 위치의 모든 하위 파일을 출력할 수 있습니다.

위 코드와 같은 경우에는 특정 확장자만 필터를 추가했습니다.

```python
from fs.copy import copy_fs
copy_fs('./text', 'zip://text.zip')
```

copy_fs를 가져오면 파일들을 zip 파일로 압축할 수 있습니다.
