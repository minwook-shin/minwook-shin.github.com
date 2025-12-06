---
layout: post
title: Docker와 Docker-compose 알아보기
---

오늘은 Docker와 Docker-compose 에 대하여 알아봅니다.

## 개요

Docker, Docker-compose는 컨테이너 가상화 기술로 수 많은 컨테이너들을 쉽게 배포할 수 있게 해줍니다.

## Docker 설치

우분투에 Docker를 설치합니다.

```
sudo apt-get remove docker docker-engine docker.io containerd runc
```

docker, docker.io , or docker-engine 에 대한 오래된 버전을 제거합니다.

```
sudo apt-get update
```

패키지 저장소 인덱스를 업데이트합니다.

```
sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
```

HTTPS를 위한 패키지를 설치합니다.

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

도커의 공식 GPG 키를 추가합니다.

```
sudo apt-get update
```

패키지 저장소 인덱스를 다시 업데이트합니다.

```
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

도커 엔진을 설치합니다.

```
sudo docker run hello-world
```

hello-world 이미지로 컨테이너를 만들어봅니다.


만약 맥이나 윈도우에서 도커를 수행하려면 아래 링크에서 Docker Desktop을 받습니다.

https://docs.docker.com/docker-for-mac/install/
https://docs.docker.com/docker-for-windows/install/


## Docker 제거 

```
sudo apt-get purge docker-ce
```

docker-ce를 제거합니다.

```
sudo rm -rf /var/lib/docker
```

이미지와 컨테이너를 제거하려면 해당 폴더도 제거합니다.

## Docker compose 설치

Docker compose도 설치합니다.

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

curl로 1.25.0 버전의 Docker compose를 설치합니다.

```
sudo chmod +x /usr/local/bin/docker-compose
```

바이너리 파일의 실행 권한을 추가합니다.

```
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

Docker compose 폴더를 링크합니다.

```
sudo curl -L https://raw.githubusercontent.com/docker/compose/1.25.0/contrib/completion/bash/docker-compose -o /etc/bash_completion.d/docker-compose
```

bash의 자동완성을 위해서 위 명령을 수행합니다.


맥과 윈도우는 Docker Desktop에 포함되어 있습니다.

## Docker compose 제거 

```
sudo rm /usr/local/bin/docker-compose
```

docker-compose를 제거합니다.

## 예제

```Dockerfile
FROM python:3.7.5-slim-buster

COPY . /app

WORKDIR /app
```

python 이미지를 가져와서 소스 파일을 복사한 도커 파일을 만듭니다.

```yml
services:
  python_command1:
    build:
      context: .
      dockerfile: Dockerfile
    command: python3 example1.py
    restart: always
  python_command2:
    build:
      context: .
      dockerfile: Dockerfile
    command: python3 example2.py
    restart: always
```

docker-compose 파일로 여러 서비스들을 같이 배포합니다.

docker-compose build로 빌드하거나, docker-compose up으로 빌드와 실행을 할 수 있습니다.