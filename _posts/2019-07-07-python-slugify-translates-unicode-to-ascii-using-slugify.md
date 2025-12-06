---
layout: post
title: Python 유니코드를 ASCII로 바꾸는 slugify 라이브러리 알아보기
---

오늘은 Python에서 유니코드를 ASCII로 바꾸는 slug화 시킬 수 있는 slugify 라이브러리를 알아보려 합니다.

## slugify 설치

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
pip install python-slugify
```

pip로 slugify를 설치합니다.

## 예제

```python
from slugify import slugify
```

slugify를 가져옵니다.

```python
txt = 'hello &amp; world!'
r = slugify(txt)
print(r)

txt = 'you are #1'
r = slugify(txt)
print(r)

txt = "___This is a test ---"
r = slugify(txt)
print(r)
txt = "This -- is a ## test ---"
r = slugify(txt)
print(r)
```

slugify로 유니코드 텍스트를 아스키로 변환하여 URL에서 작동할 수 있도록 만들 수 있습니다.

```python
txt = 'one two three four five'
r = slugify(txt, max_length=17)
print(r)
txt = 'one two three four five'
r = slugify(txt, max_length=18, word_boundary=True)
print(r)

txt = 'one two three four five'
r = slugify(txt, max_length=20, word_boundary=True, separator=".")
print(r)
```

max_length로 길이를 제한할 수 있으며 단어 단위로 끊을 수도 있습니다.

```python
txt = 'one two three four'
r = slugify(txt, max_length=12, word_boundary=True, save_order=True)
print(r)
txt = 'one two three four'
r = slugify(txt, max_length=12, word_boundary=True, save_order=False)
print(r)
```

save_order가 참이면 아무로 단어의 순서를 그대로 유지한 채로 자릅니다.

```python
txt = 'hello, stop world!'
r = slugify(txt, stopwords=['stop'])
print(r)
```

특정 단어를 제외할 수 있습니다.

```python
txt = "_Passed 1 test_"
regex_pattern = r'[^-a-z_]+'
r = slugify(txt, regex_pattern=regex_pattern)
print(r)
```

정규 표현식으로 표현하고 싶은 것만 출력할 수 있습니다.

위 코드 같은 경우에는 숫자만 지워지게 됩니다.

```python
txt = 'python | java & go'
r = slugify(txt, replacements=[['|', 'or'], ['&', 'and']])
print(r)
```

특정 텍스트를 치환할 수 있습니다.