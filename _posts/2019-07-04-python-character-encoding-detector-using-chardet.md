---
layout: post
title: Python 문자열의 인코딩을 인식하는 Chardet 라이브러리 알아보기
---

오늘은 Python에서 웹사이트 혹은 파일에서 문자열을 가져와서 인코딩을 인식하는 Chardet 라이브러리를 알아보려 합니다.

## Chardet 설치

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
pip install chardet
```

pip로 Chardet을 설치합니다.

## detect 예제

```python
import chardet
```

chardet을 가져옵니다.

```python
import urllib.request
```

사이트의 인코딩을 알기 위한 urllib.request도 가져옵니다.

```python
with urllib.request.urlopen('https://google.com') as f:
    response = f.read()

output = chardet.detect(response)
print(output)
```

chardet.detect로 google의 사이트 인코딩을 알 수 있습니다.

ascii로 출력됩니다.

```python
with urllib.request.urlopen('https://naver.com') as f:
    response = f.read()

output = chardet.detect(response)
print(output)
```

chardet.detect로 naver의 사이트 인코딩을 알 수 있습니다.

utf-8로 출력됩니다.

```python
with urllib.request.urlopen('http://www.auction.co.kr') as f:
    response = f.read()

output = chardet.detect(response)
print(output)
```

chardet.detect로 auction의 사이트 인코딩을 알 수 있습니다.

EUC-KR로 출력되며 언어가 Korean이 별도의 키 값으로 출력됩니다.

## UniversalDetector 예제

여러 텍스트의 인코딩을 알아내려면 UniversalDetector 객체를 이용합니다.

여러 번 detect.feed를 호출한 다음 detector.result로 인코딩을 확인할 수 있습니다.

```python
from chardet.universaldetector import UniversalDetector
```

UniversalDetector를 가져옵니다.

```python
detector = UniversalDetector()

with urllib.request.urlopen('http://www.auction.co.kr') as f:
    response = f
    for line in response.readlines():
        detector.feed(line)
        if detector.done:
            break
detector.close()

print(detector.result)
```

auction의 사이트 문자열을 계속 가져오면서 feed를 호출합니다.

detector.done이 참으로 나타나면 멈추고, UniversalDetector 객체를 닫은 다음, 결과를 출력합니다. 

## 명령어

chardetect 명령어를 이용하여 interactive shell을 쓸 수 있습니다.

CTRL-D를 이용하여 그동안 작성한 텍스트의 인코딩을 확인할 수 있습니다.
