---
layout: post
title: Python Pyright를 VS Code에 사용해보기
---

오늘은 Python 정적 타입 검사기인 Pyright를 직접 사용해보려 합니다.

## 개요

Pyright는 정적 타입 검사기로서 mypy와 같은 기존 Python 타입 검사기의 문제들을 해결하기 위해 나왔습니다.

## 다른 점

우선 속도는 5배 이상 빠르다고 나와있으며, 파일을 수정했을 경우에는 더 빠르게 된다고 합니다.

그리고 타입스크립트로 작성되어 노드 위에서 구동되기 때문에 pip를 이용하여 별도로 설치할 필요가 없다고 합니다.

## 지원하는 pep 규칙

- PEP 484

- PEP 526

- PEP 544

## 지원하는 파이썬 버전

```
Pyright currently provides support for Python 3.0 and newer. There is currently no plan to support older versions.
```

python 3.0 이상만 지원하고, 구 버전(2.X)은 지원하지 않는다고 합니다.

## VS code 확장 설치

비주얼스튜디오 코드는 확장 프로그램으로만 설치해도 작동합니다.

[VS code 마켓 플레이스](https://marketplace.visualstudio.com/items?itemName=ms-pyright.pyright&ssr=false#overview)에서 내려받거나, VS code 에디터 상의 확장 탭에서 ms-pyright.pyright를 직접 검색해서 설치해도 됩니다.

## npm 설치

```
npm i -g pyright
```

npm을 이용하여 전역으로 설치할 수도 있습니다.

## 지원하는 명령어 인자

- -h,--help

- -P,--python-path DIRECTORY

- -p,--project FILE OR DIRECTORY

- -t,--typeshed-path DIRECTORY

- -v,--venv-path DIRECTORY

- -w,--watch

## 설정

파이썬 코드를 작성할 폴더의 최상단에 pyrightconfig.json으로 새 파일을 만들어 줍니다.

```json
{
  "include": ["src"],
  "exclude": ["doc"],

  "typingsPath": "src/typestubs",

  "reportTypeshedErrors": true,
  "reportMissingImports": false,

  "pythonVersion": "3.7",
  "pythonPlatform": "Darwin",

  "executionEnvironments": [
    {
      "root": "src/sdk",
      "pythonVersion": "3.0",
      "extraPaths": ["src/backend"]
    },

    {
      "root": "src"
    }
  ]
}
```

include, exclude는 포함되어야 하거나 포함되지 않아야 하는 경로입니다.

typingsPath는 타입 스텁 파일이 들어있는 경로입니다.

reportTypeshedErrors, reportMissingImports는 경고 또는 오류를 출력하는 것을 제어하는 것으로서 타입 스텁 파일에 대한 진단이나, import에 대한 진단을 중지합니다.

pythonVersion, pythonPlatform은 파이썬의 버전이나 플랫폼을 지정할 수 있습니다.

executionEnvironments는 실행 환경을 정의할 수 있습니다.

## 실험

```python
class Test(object):
    def __init__(self):
        self.hello: str = ""

    def __get__(self, instance, e) -> str:
        return instance.hello

    def __set__(self, instance, value) -> None:
        instance.hello = value
```

Test라는 클래스를 만들어보았습니다.

함수에는 반환 타입을 명시해주고, 속성에도 타입을 명시했습니다.

이렇게 되면 VS code에서도 타입 검사가 수시로 이루어지게 됩니다.

```python
t = Test()
t.hello = "hello, world!"
print(t.hello)
```

Test 객체를 만들어서 getter와 setter를 동작시켰습니다.

Test 객체의 hello라는 변수가 에디터에서 (variable) hello: str라고 표기됨을 알 수 있습니다.

```python
def testStrFunction() -> int:
    return "hello, world!"
```

만약 반환 타입을 틀리게 작성하면 반환 부분에 빨간 라인이 생성되며 아래와 같이 오류를 출력합니다.

Expression of type 'str' cannot be assigned to return type 'int'

```python
def testStrFunction() -> str:
    return "hello, world!"
```

int를 str로 변경하면 오류는 제거됩니다.

## 마무리

해당 정적 타입 검사기는 2019년 3월 22일에 릴리즈된 만큼, 현재 글을 작성한 시기(2019년 3월 25일)보다 많은 기능이 추가될 수 있고, 설치 방식이 달라졌을 수도 있습니다.

꼭 https://github.com/Microsoft/pyright 를 확인해보고 사용하시기 바랍니다.
