---
layout: post
title: Python URL을 파싱하고 조작할 수 있는 furl 라이브러리 알아보기
---

오늘은 Python에서 URL을 파싱하고 조작할 수 있는 furl 라이브러리를 알아보려 합니다.

## furl 설치

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
pip install furl
```

pip로 furl을 설치합니다.

## 예제

```python
from furl import furl
```

furl을 가져옵니다.

```python
f = furl('https://www.google.co.kr/search?source=hp&q=google')
f.args['test'] = 'test'
del f.args['source']
print(f.url)
```

딕셔너리 형식으로 쿼리를 추가하거나 del 키워드로 삭제할 수 있습니다.

```python
print(furl('https://www.google.co.kr/search?source=hp&q=google').add({'test': 'test'}).url)
```

인라인으로 url의 쿼리를 추가할 수 있습니다.

```python
print(furl('https://www.google.co.kr/search?source=hp&q=google').set({'q': 'google'}).url)
```

인라인으로 url의 쿼리를 설정할 수 있습니다.

```python
print(furl('https://www.google.co.kr/search?source=hp&q=google').remove(['source']).url)
```

인라인으로 url의 쿼리를 제거할 수 있습니다.

```python
f = furl('http://www.google.com/')
f.fragment.path.segments = ['first', 'directories']
f.fragment.args = {'test': 'test'}
print(f.url)
```

fragment로도 쿼리를 설정할 수 있습니다.

```python
f = furl('http://www.google.com/path?q=google')
print(f.copy().remove(path=True).url)
print(f.copy().set(host='host.com').url)
print(f.copy().join('/index.html').url)
print(f.copy().add(fragment_path='add_fragment').url)
```

url을 복사해서 원본을 수정하지 않고도 조작할 수 있습니다.

```python
print(f.asdict())
```

원본을 딕셔너리로 출력하면 처음 furl 객체를 생성할 때 넣었던 url 그대로 존재함을 알 수 있습니다.