---
layout: post
title: virtualenv 알아보고 Python 프로젝트에 적용하기
---

오늘은 복수의 Python 프로그램을 만들 때에 전역적으로 설치되는 패키지들을 가상 환경인 virtualenv으로 나누어주는 과정을 실습해보려 합니다.

## virtualenv

프로젝트마다 다른 Python 의존성 패키지들을 각각의 독립적인 프로젝트 폴더에 따로 제공해줄 수 있습니다.

## 예시

예를 들자면, 이전에 진행한 Python 프로젝트에 rx 1.x 버전이 설치되어있어도 새로운 Python 프로젝트에서는 rx 3.x 버전을 설치해도 손쉽게 분리할 수 있다는 것입니다.

```
pip3 install rx
```

A 프로젝트에서는 위와 같이 작성하고,

```
pip3 install -pre rx
```

B 프로젝트에서는 위와 같이 작성해도 별개의 프로젝트의 virtualenv 폴더로 나누어지게 됩니다.

이와 같이 할 수 있는 점은 virtualenv 폴더에 pip 패키지 내용이 복사되므로 가능한 점입니다.

## virtualenv 설치하기

```
pip3 install virtualenv
```

## 가상환경 만들기

```
virtualenv env --distribute
```

## 가상환경 활성화하기

```
source env/bin/activate
```

```
deactivate
```

## 가상환경에 패키지 설치하기

```
pip3 install rx
```

```
pip3 install -r requirements.txt
```

## vscode 설정

에디터의 좌측 아래에 보면 알겠지만, Python 환경을 선택하는 매뉴 중에서 로컬 폴더의 virtualenv 폴더를 선택해주면 됩니다.

그러면 기존 Python 확장에 포함되어있는 pylint와 autopep8 패키지가 인식되지 않는다고 오류가 출력됩니다.

그러면 터미널에서 해당 python 환경에서

```
pip3 install pylint
```

```
pip3 install autopep8
```

으로 설치해주면 됩니다.

```txt
pylint
autopep8
```

혹은 위와 같이 requirements-dev.txt 파일을 작성한 뒤에

```
pip3 install -r requirements-dev.txt
```

pip3 명령어로 한번에 설치해주면 됩니다.

## 그 외

pipenv라고 pip와 virtualenv를 같이 사용할 수 있는 오픈소스 툴이 있습니다.
