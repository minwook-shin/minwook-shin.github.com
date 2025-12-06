---
layout: post
title: Google cloud platform Compute engine에 private pypi 서버 올리기
---

오늘은 구글 클라우드 플랫폼의 컴퓨트 엔진에 private pypi 서버를 사용할 수 있게 설정해서 구동해보려 합니다.

private pypi 서버는 도커로 설치할 수 있지만, 이번 포스트에서는 pip로 직접 설치했습니다.

테스트에는 무료 등급에 존재하는 컴퓨트 엔진의 비선점형 f1-micro VM 인스턴스를 사용했습니다.

us-west1, us-central1, us-east1가 해당되며, 월 하나의 vm만 무료 입니다.

https://cloud.google.com/free/docs/gcp-free-tier?hl=ko

## gcp

https://console.cloud.google.com/ 에 접속하여 프로젝트를 만들고 f1-micro VM 인스턴스를 만듭니다.

```bash
gcloud compute firewall-rules create rule-allow-tcp-8080 --source-ranges 0.0.0.0/0 --target-tags allow-tcp-8080 --allow tcp:8080
```

클라우드 콘솔 사이트 상단에 cloud shell 활성화를 클릭하여 Google Cloud Shell 머신으로 위 명령어를 입력합니다.

방화벽 규칙을 추가해서 8080 포트를 열 수 있습니다.

```bash
gcloud compute instances add-tags \[VM_NAME\] --tags allow-tcp-8080
```

만든 f1-micro VM 인스턴스의 이름을 넣고 위 명령어를 추가하면 해당 인스턴스에 allow-tcp-8080 규칙을 적용할 수 있습니다.

## server

이제 인스턴스에 ssh로 접속하여 서버에서 작업을 해야 합니다.

```
apt-get install python3-pip
```

처음 우분투로 이미지를 만들면 pip가 설치되어 있지 않을 수도 있으므로 pip를 설치합니다.

```bash
pip3 install pypiserver
mkdir ~/packages
pypi-server -p 8080 ~/packages
```

pypiserver를 설치하고, 패키지를 저장할 폴더를 만들고 지정합니다.

http://(외부 IP):8080 으로 들어가면 pypi 서버가 수행됨을 확인할 수 있습니다.

```bash
pip install passlib
sudo apt install apache2-utils
htpasswd -sc .htaccess (user name)
```

이제 패키지를 업로드(혹은 업데이트)하고 다운로드할 때에 입력할 비밀번호를 지정해야 합니다.

passlib를 설치하고, apache2-utils를 설치하라고 안내가 출력되면 apache2-utils까지 설치합니다.

.htaccess 파일을 만들어서 사용자 이름과 비밀번호를 생성합니다.

```bash
pypi-server -p 8080 -P .htaccess -a update,list,download -v ~/packages
```

'list', 'download', 'update' 행동을 지정하여 접근할 때에 인증을 요구할 수 있습니다.

추가로 -v 인자를 추가하면 로그를 출력할 수 있으며, --disable-fallback 을 추가하면 private pypi 서버에 존재하지 않는 패키지를 pypi 서버에 바로 연결하지 않습니다.

## client

패키지를 업로드하기 위해서 pip를 사용할 컴퓨터의 홈 폴더에 ~/.pypirc 파일을 생성합니다.

```rc
[distutils]
index-servers =
  pypi
  internal

[pypi]
username:<pypi_username>
password:<pypi_passwd>

[internal]
repository: http://(외부 IP):8080
username: <username>
password: <passwd>
```

internal 부분의 repository, username 그리고 password를 자신의 서버에 맞게 작성합니다.

## upload

파이썬 패키지 구조로 되어있고 setup.py가 작성되있는 폴더에서 아래와 같이 입력합니다.

```bash
python3 setup.py sdist upload -r internal
```

tar.gz 파일로 업로드 하거나,

```bash
python3 setup.py bdist_wheel upload -r internal
```

wheel 파일로 업로드할 수 있습니다.

## search

```bash
pip3 search -i http://(외부 IP):8080 검색어
```

특정 pypi 서버에 있는 패키지를 조회할 수 있습니다.

## install

```bash
pip3 install --extra-index-url http://(외부 IP):8080 --trusted-host 34.66.144.188 python_package_name
```

extra-index-url로 private pypi 서버의 패키지를 내려받을 수 있습니다.

다만, https가 아니면 trusted-host도 추가로 부여해야 합니다.

## uninstall

```bash
pip3 uninstall python_package_name
```

패키지를 지울 때에는 기존 pypi와 동일합니다.
