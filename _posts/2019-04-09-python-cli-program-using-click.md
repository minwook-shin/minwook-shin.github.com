---
layout: post
title: Python click 패키지를 사용하여 cli 프로그램 만들기
---

오늘은 Python에서의 command line interface 툴킷인 click 패키지에 대하여 알아보려 합니다.

## 개요

파이썬 데코레이터로 인하여 command line interface 프로그램을 만들 수 있습니다.

## click 설치

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
pip install click
```

pip로 click을 설치합니다.

## 문서

```python
import click


@click.command()
def hello():
    """This script prints hello."""
    click.echo('Hello World!')


if __name__ == '__main__':
    hello()
```

doc string으로 사용법을 명시할 수 있습니다.

```
Usage: main.py [OPTIONS]

  This script prints hello.

Options:
  --help  Show this message and exit.
```

위와 같이 터미널에 작성해야 합니다.

## command

```python
import click


@click.group()
def cli():
    pass


@click.command()
def add_user():
    click.echo('Added User.')


cli.add_command(add_user)

if __name__ == '__main__':
    cli()
```

command를 추가할 수 있습니다.

```
Usage: main.py [OPTIONS] COMMAND [ARGS]...

Options:
  --help  Show this message and exit.

Commands:
  add-user
```

위와 같이 터미널에 작성해야 합니다.

## parameter

```python
import click


@click.command()
@click.argument('name')
def hello(name):
    click.echo('Hello %s!' % name)


if __name__ == '__main__':
    hello()
```

인자를 통해 값을 받아올 수 있습니다.

```
Usage: main.py [OPTIONS] NAME

Options:
  --help  Show this message and exit.
```

위와 같이 터미널에 작성해야 합니다.

## option

```python
import click


@click.command()
@click.option('--url', '-u', 'arg', required=True, type=str)
def url_check(arg):
    click.echo('URL: %s' % arg)


if __name__ == '__main__':
    url_check()
```

옵션 이름이나 타입, 그리고 필수로 작성해야 하는지에 대한 여부 등을 설정할 수 있습니다.

```
Usage: main.py [OPTIONS]

Options:
  -u, --url TEXT  [required]
  --help          Show this message and exit.
```

위와 같이 터미널에 작성해야 합니다.

## input

```python
import click

value = click.prompt('Please enter a integer', type=int)

try:
    click.confirm('Do you want to continue?', abort=True)
except click.exceptions.Abort as e:
    pass
```

prompt와 confirm을 통해 사용자의 입력을 받을 수 있습니다.

confirm에서 abort가 참일 경우에는 click.exceptions.Abort 오류가 발생합니다.
