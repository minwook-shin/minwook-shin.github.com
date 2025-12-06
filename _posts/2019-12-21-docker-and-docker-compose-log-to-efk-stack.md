---
layout: post
title: Docker log 들을 EFK 스택에 보내는 방법 알아보기
---

오늘은 Docker와 Docker-compose로 Docker 컨테이너에서 나오는 log 들을 EFK 스택에 보내는 방법을 대하여 알아봅니다.

## 개요

Docker 컨테이너에서 나오는 로그를 EFK (Elasticsearch + Fluentd + Kibana) 스택에 수집할 수 있습니다.

검색 엔진인 Elasticsearch, 로그 수집기인 Fluentd 그리고 데이터 시각화 웹 UI인 Kibana를 같이 묶어서 부르는 게 EFK 스택입니다.

## 도커 설치

이전 포스트에서 Docker와 Docker-compose를 설치하는 내용을 볼 수 있습니다.

## docker-compose 구성

이번 예시에서는 파이썬 컨테이너와 fluentd, elasticsearch, kibana를 docker-compose.yml 파일로 구성합니다.

```yml
version: '3.2'
services:
  python_command1:
    build:
      context: .
      dockerfile: Dockerfile
    command: python3 example1.py
    restart: always
    logging:
      driver: "fluentd"
      options:
        fluentd-address: localhost:24224
        tag: python.command
```

컨테이너 로깅 드라이버를 Fluentd Logging Driver로 지정합니다.

이제 로그가 발생하면 fluentd-address로 보냅니다.

```yml
  fluentd:
    build: ./fluentd
    volumes:
      - ./fluentd/conf:/fluentd/etc
    links:
      - "elasticsearch"
    ports:
      - "24224:24224"
      - "24224:24224/udp"
```

fluent에서 받는 포트를 열고 도커 이미지, 볼륨을 준비합니다.

```yml
  elasticsearch:
    image: elasticsearch
    expose:
      - 9200
    ports:
      - "9200:9200"
```

elasticsearch 이미지와 포트를 준비합니다.

```yml
  kibana:
    image: kibana
    links:
      - "elasticsearch"
    ports:
      - "5601:5601"
```

localhost의 5601 포트로 kibana를 사용할 수 있게 준비합니다.


## fluentd Dockerfile 구성

```Dockerfile
FROM fluent/fluentd:v0.12-debian
RUN ["gem", "install", "fluent-plugin-elasticsearch", "--no-rdoc", "--no-ri", "--version", "1.9.2"]
```

fluent/fluentd 공식 이미지로 fluentd의 Dockerfile 파일을 준비합니다.

## fluent.conf 구성

```conf
<source>
  @type forward
  port 24224
  bind 0.0.0.0
</source>
<match *.**>
  @type copy
  <store>
    @type elasticsearch
    host elasticsearch
    port 9200
    logstash_format true
    logstash_prefix fluentd
    logstash_dateformat %Y%m%d
    include_tag_key true
    type_name access_log
    tag_key @log_name
    flush_interval 1s
  </store>
</match>
```

forward로 입력하고, elasticsearch로 출력하는 fluent의 설정 파일을 만듭니다.

## 실행

docker-compose up -d 로 서비스를 빌드하고 실행합니다.

http://localhost:5601/ 주소에 접속하고 인덱스 패턴을 fluentd-* 로 설정하면 fluentd에서 받는 모든 로그가 Kibana로 출력됩니다.