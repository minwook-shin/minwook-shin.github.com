---
layout: post
title: Docker 기반 PostgreSQL 데이터베이스 구축하기
---

오늘은 Docker로 PostgreSQL 컨테이너를 구동해서 간단히 데이터베이스 구축해보려고 합니다.

여기에 Docker-compose 로 더 쉽게 작업해보겠습니다.

# 개요

코드에서 데이터베이스 관련 테스트를 진행할 때에 상용 서버의 데이터베이스를 건드리기는 쉽지 않습니다.

데이터베이스를 직접 로컬에 설치하기에는 테스트를 위해서 더 시간을 들여야 되는지 의문이 생기기도 합니다.

그래서 데이터베이스를 도커 컨테이너로 올려두고 접속해서 자유롭게 사용해보려 합니다.

수 많은 RDB 또는 NoSQL 종류가 존재하지만 오늘은 PostgreSQL 데이터베이스를 선택해서 실습을 진행하려고 합니다.

## 도커 설치

우선 우분투에 Docker를 설치합니다. 이미 설치되어 있다면 넘기셔도 됩니다.

```
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates curl gnupg lsb-release
```

패키지 저장소 인덱스를 업데이트하고 사전에 필요한 의존성 패키지를 설치합니다.

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

도커의 공식 GPG 키를 추가하고 stable 채널로 설정합니다.

```
sudo apt-get update
```

도커 소스 리스트를 반영하기 위하여 패키지 저장소 인덱스를 다시 업데이트합니다.

```
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

도커 엔진을 설치합니다.

만약 맥이나 윈도우에서 도커를 수행하려면 아래 링크에서 Docker Desktop을 받습니다.

https://docs.docker.com/docker-for-mac/install/
https://docs.docker.com/docker-for-windows/install/

## Docker compose 설치

Docker compose도 설치합니다.

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

curl로 Docker compose를 설치합니다.

```
sudo chmod +x /usr/local/bin/docker-compose
```

바이너리 파일의 실행 권한을 추가합니다.

```
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

Docker compose 폴더를 링크합니다.

맥과 윈도우는 Docker Desktop에 포함되어 있으며, 최근에는 일부 명령에 대하여 docker compose 명령어도 지원할 만큼 통합되어가고 있습니다.

## docker-compose 구성

PostgreSQL 데이터베이스를 컨테이너로 실행하기 위한 정보를 docker-compose.yml 파일로 입력합니다.

docker-compose.yml 파일명이 아니라도 상관없지만, 나중에 실행할 때 파일명을 수동으로 입력해야 합니다.

```yml
version: '3.8'
services:
  postgres:
    image: postgres:13.2
    restart: always
    environment:
      POSTGRES_PASSWORD: password
      POSTGRES_USER: admin
      POSTGRES_DB: content
      POSTGRES_INITDB_ARGS: --encoding=UTF-8
    volumes:
    - ./pg_data:/var/lib/postgresql/data
    - ./init-user-db.sh:/docker-entrypoint-initdb.d/init-user-db.sh
    ports:
    - 5432:5432
```

현시점의 최신 버전인 13.2 postgres 이미지를 기반으로 항상 재시작하도록 작성하고, 비밀번호, 사용자 이름, 데이터베이스 등을 환경변수로 입력합니다.

여기애서 사용자 이름이나 비밀번호는 env 파일로 분리하거나 환경 변수로 미리 등록하여 직접 작성 안 하시는 것도 좋습니다.

```/var/lib/postgresql/data``` 경로를 볼륨으로 지정하면 컨테이너가 내려가도 로컬 파일로 남게되며, init-user-db.sh 파일을 지정하면 처음 데이터베이스가 설정될 때 수행되는 스크립트를 지정할 수 있습니다.

마지막으로 포트를 지정해서 기본 포트 5432 맵핑합니다.

```yml
  adminer:
    image: adminer
    restart: always
    ports:
      - 8080:8080
```

Adminer로 간단하게 방금 전에 설정한 데이터베이스를 웹 상으로 볼 수 있습니다. 

Adminer는 PHPmyAdmin보다 가볍게 구동할 수 있습니다.

같은 도커 데몬 안에 존재하므로 postgres:5432/content 주소로 서버 접근할 수 있습니다.

```bash
#!/bin/bash
set -e

psql -v ON_ERROR_STOP=1 --username "$POSTGRES_USER" --dbname "$POSTGRES_DB" <<-EOSQL
    CREATE TABLE article (
    id SERIAL,
    description VARCHAR(256) NOT NULL
);
EOSQL
```

만약 데이터베이스 컨테이너를 처음 수행하면 위 init-user-db.sh 파일이 수행되게 되는데, 스크립트로 테이블을 미리 만들 수 있습니다.

```
chmod +x init-user-db.sh
```

그리고 해당 스크립트가 권한 이슈를 발생시킨다면, 실행 가능하도록 변경하고 다시 컨테이너를 올리면 해결됩니다.

## 실행

docker-compose up -d 로 postgres 데이터베이스 이미지를 도커 저장소로부터 내려받고 실행합니다.

```sql
INSERT INTO article
(
    description
)
VALUES (
           '테스트'
       );
```

```sql
select * from article
limit 10
```

간단하게 값을 넣고, 조회하면 컨테이너가 잘 구동하는 것을 확인할 수 있습니다.

만약 jdbc 클라이언트로 제어하고 싶다면 jdbc:postgresql://localhost:5432/content 주소로 접근해서 사용할 수 있습니다.
