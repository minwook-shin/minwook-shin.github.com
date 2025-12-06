---
layout: post
title: 파이썬 버전 관리 pyenv 알아보기
---

오늘은 파이썬의 버전들을 한 시스템 내에서 관리 할 수 있는 pyenv에 대하여 알아봅니다.

## 개요

pyenv로 여러 버전의 Python 프로젝트 별로 관려하고, 환경 변수를 사용하여 Python 버전을 대체 할 수 있습니다.

루비의 rbenv에서 파생되었다고 합니다.

pyenv는 쉘 스크립트로 만들어졌고, pyenv-virtualenv와 같이 설치하면 virtualenv도 사용할 수 있다고 합니다.

## 설치

```sh
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
```

$HOME/.pyenv 위치에 pyenv 소스 코드를 받습니다.

```sh
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc

echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshenv
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshenv
```

환경 변수 PYENV_ROOT를 등록해야 합니다.

bash를 사용하면 윗 두 줄만 수행하며, 만약 zsh을 사용하면 아래 두 줄만 수행합니다.

```sh
brew update
brew install pyenv
```

만약 맥 운영체제라면 위 git clone을 하지 않고, brew로 할 수 있습니다.

```sh
echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.bashrc

echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.zshenv
```

pyenv init을 위해 bash를 사용하면 윗 줄만 수행하며, 만약 zsh을 사용하면 아랫 줄만 수행합니다.

```sh
$ exec "$SHELL"
```

쉘을 재시작합니다.

```sh
$ pyenv install 3.7.5
```

pyenv를 설치하면 파이썬 버전을 설치할 수 있습니다.

## 업그레이드

```sh
cd $(pyenv root)
git pull
```

git 기반이기 때문에 git pull로 업그레이드할 수 있습니다.

## 제거 

```sh
rm -rf $(pyenv root)
```

pyenv 폴더를 제거합니다.

```sh
brew uninstall pyenv
```

만약 brew로 설치했다면 brew uninstall 로 제거합니다.

## 커맨드

pyenv에 사용할 수 있는 커맨드는 아래와 같습니다.

* pyenv local

* pyenv global

* pyenv shell

* pyenv install

* pyenv uninstall

* pyenv rehash

* pyenv version

* pyenv versions

* pyenv which

* pyenv whence

https://github.com/pyenv/pyenv/blob/master/COMMANDS.md

## 가상 환경

pyenv-virtualenv라는 pyenv 플러그인은 pyenv에서 생성한 가상 환경을 관리 할 수 ​​있도록 다양한 기능이 제공됩니다. 

```sh
git clone https://github.com/pyenv/pyenv-virtualenv.git $(pyenv root)/plugins/pyenv-virtualenv
```

pyenv-virtualenv를 pyenv 플러그린 폴더에 git clone 받습니다.

```sh
echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bash_profile

echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.zshenv
```

pyenv virtualenv-init을 위해 bash를 사용하면 윗 줄만 수행하며, 만약 zsh을 사용하면 아랫 줄만 수행합니다.

```sh
exec "$SHELL"
```

쉘을 재시작합니다.

```sh
brew install pyenv-virtualenv
```

만약 맥 운영체제라면 위 git clone을 하지 않고, brew로 할 수 있습니다.

```sh
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```

맥 운영체제라서 brew로 설치했다면 pyenv virtualenv-init을 해줍니다.

```sh
pyenv virtualenv 3.7.5 venv
```

이제 특정 venv를 생성할 수 있습니다.

```sh
pyenv activate venv
pyenv deactivate
```

그리고 만든 venv 이름으로 activate할 수 있고, 다시 돌아가려면 deactivate합니다.