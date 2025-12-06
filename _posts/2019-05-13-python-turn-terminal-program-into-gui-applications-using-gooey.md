---
layout: post
title: Python 터미널 프로그램을 GUI로 전환하는 Gooey 라이브러리 알아보기
---

오늘은 Python으로 만들어진 터미널 프로그램을 GUI 그래픽으로 전환할 수 있는 라이브러리인 Gooey를 적용해보려 합니다.

## Gooey 설치

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
pip install Gooey
```

pip로 Gooey를 설치합니다.

## Hello world

```python
from gooey import Gooey, GooeyParser
```

Gooey를 가져옵니다.

```python
@Gooey()
def main():
    parser = GooeyParser(description='HELLO WORLD.')
```

GooeyParser로 gui에서 값을 파싱할 준비를 합니다.

```python
    parser.add_argument(
        'string_field',
        metavar='enter string',
        help='hello, world!')
```

터미널에서 인자를 추가하듯이 인자를 추가합니다.

인자의 이름을 정하고, 기본 값을 추가할 수 있습니다.

```python
    args = parser.parse_args()
    print(args.string_field)
```

인자의 이름을 조회하면 프로그램이 실행될 때에 입력한대로 출력됩니다.

```python
if __name__ == '__main__':
    main()
```

main 함수를 실행합니다.

## Widget

```python
from gooey import Gooey, GooeyParser
```

Gooey를 가져옵니다.

```python
@Gooey(program_name="Widget")
def main():
    parser = GooeyParser(description="widgets Example ")
```

GooeyParser로 gui에서 값을 파싱할 준비를 합니다.

```python
    parser.add_argument("directory")
    parser.add_argument(
        "FileChooser", widget="FileChooser")
    parser.add_argument(
        "DirectoryChooser", widget="DirChooser")
    parser.add_argument(
        "FileSaver", widget="FileSaver")
    parser.add_argument(
        "MultiFileSaver", widget="MultiFileChooser")
```

일반적인 문자열을 받는 위젯부터 파일 또는 디렉토리의 위치를 지정할 수 있는 Chooser, Saver위젯까지 만들 수 있습니다.

```python
    parser.add_argument('-c', '--count', action='count')
    parser.add_argument("-s", "--store-true", action="store_true")
    parser.add_argument("-i", "--input", default="default string")
    parser.add_argument('-t', '--type', default=0, type=int)
    parser.add_argument('-d', '--dateChooser', type=int, widget='DateChooser')
    parser.add_argument('-o', '--choices', choices=['yes', 'no'])
```

인자의 이름이 지정되면 선택적인 입력으로 간주되어 꼭 입력하지 않아도 됩니다.

액션에는 count, store_true 등이 있으며, 이 역시 기본 값을 지정할 수 있습니다.

타입을 지정하면 해당 타입으로 강제할 수 있습니다.

```python
    group = parser.add_mutually_exclusive_group()
    group.add_argument('-g1', '--group-1', dest='group1', action="store_true")
    group.add_argument('-g2', '--group-2', dest='group2', action="store_true")
```

그룹을 만들 수 있습니다.

```python
    args = parser.parse_args()
```

프로그램이 실행될 때에 인자의 값을 받습니다.

```python
if __name__ == '__main__':
    main()
```

main 함수를 실행합니다.

## progress

```python
@Gooey(progress_regex=r"^\[progress\] (\d+)$")
# disable_stop_button=True
```

progress는 특이하게 콘솔 창에 출력되는 값을 정규식 패턴으로 찾아 인식하게 됩니다.

위 코드는 \[progress\] 다음에 숫자가 오면 해당 숫자만큼 진행 바가 증가합니다.

```python
def main():
    parser = GooeyParser(prog="progress_bar example")
    _ = parser.parse_args(sys.argv[1:])
    for i in range(100):
        print("[progress] {}".format(i + 1))
        sys.stdout.flush()
        sleep(0.1)
```

print로 \[progress\] 접두사가 붙은 1부터 숫자가 출력됩니다.

```python
if __name__ == "__main__":
    main()
```

main 함수를 실행합니다.
