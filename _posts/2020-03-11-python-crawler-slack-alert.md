---
layout: post
title: GCP 인스턴스에서 파이썬 코드로 원하는 웹 데이터 가져와서 슬랙 알림받기
---

오늘은 Google cloud platform의 Computing Engine f1 인스턴스에서 파이썬으로 원하는 데이터를 원하는 간격마다 슬랙 알림받는 방법을 소개하려 합니다. 

## 서론

최근 무언가를 웹에서 찾아서 저장하거나, 자신에게 알려주는 기능을 원하는 사람들이 많아지는 것 같아서 글을 써보려 합니다.

해당 내용은 대부분 코드들이 예제 수준으로 축약되어 있으므로, 그대로 사용하는 것보다 코드를 응용하셔서 원하시는 결과에 다가갈 수 있으 좋겠습니다.

## 실습

* 파이썬 코드로 데이터 가져오기

우선 본인이 가져오고 싶은 사이트를 html 텍스트 형식으로 가져옵니다.

```python

try:
  rhtml_text = requests.get("https://example.com", timeout=30).text
except requests.exceptions.RequestException as _:
  log.info("server error")
```

위 코드는 requests 패키지로 https://example.com 사이트의 html 텍스트를 가져오는 예제입니다.

예시 사이트이므로, 다른 사이트를 가져올 때에 추가적인 헤더를 같이 첨부해야 될 수 있습니다.

* 가져온 웹 데이터에서 원하는 문자열만 추출하기

html 텍스트를 파이썬으로 가져와서 원하는 데이터만 추출하려고 합니다.

원하는 텍스트만 추출하기 위해서 parsel 패키지를 사용해봅니다.

parsel 패키지는 처음 들어보시는 분들이 많이 계시겠지만, 해당 패키지는 scrapy에서 포함되어 많이 사용되고 있었습니다.

```python
selector = Selector(text=html_text)
example_title = selector.css("body > div > h1::text").get()
if "Example Domain" in example_title:
    print("all right!")
    # send_slack(msg)
```

위 코드는 parsel 패키지의 Selector로 CSS 선택자를 이용하는 예제입니다.

CSS 선택자로 특정 태그의 문자열을 추출할 수 있습니다.

물론, CSS 선택자 외에 xpath로도 원하는 텍스트를 추출할 수 있습니다.

이 때 특정 조건으로 맞다면, 슬랙으로 알림을 보낼 수 있게 작성할 수 있습니다.

* 슬랙 웹훅 만들기

본인이 알림을 받고 싶은 슬랙 워크스페이스의 Apps에서 "Incoming WebHooks"를 검색하고 추가합니다.

Incoming WebHooks의 Integration Settings에서 설정을 마무리하면 슬랙 준비는 끝납니다.

이 부분에서 가장 중요한 것은 Webhook URL에 존재하는 주소를 기억하고 복사해놔야 합니다.

첨언하자면 Customize Name, Customize Icon으로 봇의 이름과 아이콘을 정할 수 있습니다.

* 슬랙으로 매시지 보내기

위에서 준비된 슬랙 웹훅 주소를 post로 요청하면 슬랙 메시지를 보낼 수 있습니다.

```python
requests.post(
    "https://hooks.slack.com/services/{TEXT}",
    json={
        'attachments': [
            {
                'text': f"SITE title : {example_title}",
            }
        ],
    }
)
```

위 코드는 requests 패키지에서 post 요청을 보내 본인의 워크스페이스의 채널로 텍스트가 전송되는 예제입니다.

지금까지 웹 데이터와 슬랙 전송하는 코드를 준비했습니다.

* 작성한 코드를 반복 실행할 수 있는 도커 컨테이너 만들기

이제 작성한 코드들을 도커 컨테이너로 동작하게 해야합니다.

도커는 이미 로컬에서 설치되어 있다는 가정으로 해당 포스팅을 진행합니다.

```
*/30 * * * * /usr/local/bin/python3 /main.py > /proc/1/fd/1 2>/proc/1/fd/2
```

도커 컨테이너는 각 컨테이너가 독립적으로 수행되므로 컨테이너 내부에서 cron을 사용할 수 있도록 crontab 파일을 준비합니다.

요약해서 설명하자면, crontab 스케줄과 실행문을 작성해둬야 나중에 해당 스케줄로 반복해서 실행됩니다.

참고 : https://www.raspberrypi.org/documentation/linux/usage/cron.md

crontab으로 특정 분 단위로 계속 반복하게 할 수 있다는 점을 이용하여 cron-job 이라는 파일로 저장해둡니다.

```
# 데비안 buster 기반 python 3.8.2
FROM python:3.8.2-slim-buster

# 패키지 인코딩
ENV LANG=C.UTF-8 LC_ALL=C.UTF-8

# Seoul 시간대
ENV TZ=Asia/Seoul
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# cron 설치
RUN apt-get update && \
    apt-get install --no-install-recommends -y cron && \
    apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# 소스 코드
ADD . /src

# requirements.txt 정의 (예제에서 사용하는 패키지 등)
RUN pip3 install -r /src/requirements.txt

# crontab 등록
COPY ./cron-job /etc/cron.d/cron-job
RUN chmod 0644 /etc/cron.d/cron-job
RUN crontab /etc/cron.d/cron-job

# crontab foreground 모드
CMD cron -f
```

이제부터는 도커 이미지 빌드를 위한 도커파일을 생성해봅니다.

데비안 buster 기반 파이썬 3.8.2 버전으로 시간대를 설정하고, cron 패키지를 설치하는 등의 작업으로 파이썬 반복 실행할 수 있는 환경을 만듭니다.

파이썬 코드를 복사하고 외부 패키지를 설치하며, crontab 등록 작업을 합니다.

해당 이미지로 컨테이너를 만들면 crontab foreground 모드로 계속 표준 출력과 오류를 기다리게 됩니다.

```yml
version: '3.2'

services:
  compose_process:
    build:
      context: .
      dockerfile: Dockerfile
    restart: always
```

복잡한 도커 빌드 명령어를 도커 컴포즈로 미리 작성하여 도커 컨테이너를 생성할 수 있습니다.

docker-compose 만 준비되어 있다면, docker-compose up 명령으로 컨테이너를 도커 엔진에서 구동시킬 수 있습니다.

* GCP Computing Engine f1 인스턴스에 도커 설치하고 업로드하기

지금까지 코드를 위주로 작성했으면, 이제 클라우드에 코드를 배포해보겠습니다.

http://console.cloud.google.com 구글 클라우드 플랫폼의 콘솔에 접근합니다.

Google cloud platform의 Computing Engine에서 미국 일부 지역 f1 인스턴스를 제한되는 시간(1 인스턴스 기준  무료)내에 무료로 사용할 수 있습니다. (필자 기준이며, 추후 과금에 대한 기준이 변경될 수 있습니다. 해당 내용에 대한 보증은 없습니다.)

참고 : https://cloud.google.com/free/docs/gcp-free-tier?hl=ko#always-free

인스턴스를 생성하고, ssh 클라이언트로 접속한 뒤 아래 작업을 수행합니다.

```
sudo fallocate -l 1G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```

인스턴스에 접속해서 스왑을 생성합니다.

```
htop
```

htop으로 스왑이 잘 생성되었는지 확인합니다.

```
curl -sSL https://get.docker.com | sh
sudo usermod -aG docker {user name}
exit
```

도커를 설치합니다.

```
sudo apt-get update
sudo apt-get install python3-pip
pip3 install docker-compose
```

docker-compose를 설치합니다.

```
docker-compose up
```

미리 작성한 코드와 도커 관련 파일들을 scp 복사 혹은 git 배포 방식으로 해당 인스턴스에 배포하고, docker-compose.yml 파일로 도커 컨테이너를 Computing Engine f1 인스턴스에서 구동합니다.

지금까지 파이썬 웹사이트 요청, 파싱, 슬랙 웹훅, 도커 그리고 GCP까지 간단하게 알아보았습니다. 감사합니다.