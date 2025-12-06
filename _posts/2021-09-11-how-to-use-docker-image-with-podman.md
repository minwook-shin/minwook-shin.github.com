---
layout: post
title: Mac에서 Podman 설치하고 실행하기
---

오늘은 맥에서 Podman 설치하고 도커에서 빌드한 컨테이너 이미지를 실행해보려 합니다.

## Podman

Podman은 Linux에서 OCI 컨테이너를 빌드하고 실행하기 위한 컨테이너 엔진입니다. 

도커는 도커 데몬에서 관리하지만, Podman은 기본적으로 이미지와 컨테이너 그리고 저장소가 서로 분리된 구조로 되어있습니다.

자세한 내용은 podman 문서를 참고하시면 도움이 됩니다.

https://docs.podman.io/en/latest/#what-is-podman

추가로 Podman 3.3부터 OSX와 Windows 지원이 강화되어서 클라이언트로 설정하기 편해졌습니다.

## Homebrew 준비

우선 설치하기 위해서 Homebrew를 준비합니다.

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

공식 페이지에 존재하는 Homebrew 설치 스크립트로 준비합니다.

## 설치

Homebrew로 podman을 mac에 설치합니다.

```bash
brew install podman
```

아래와 같이 Homebrew가 업데이트되면서 관련된 의존 패키지를 내려받게 됩니다.

```
Updating Homebrew...
==> Auto-updated Homebrew!
Updated 1 tap (homebrew/core).
==> Updated Formulae
Updated 10 formulae.
...
==> Installing dependencies for podman: gettext, libffi, pcre, gdbm, mpdecimal, openssl@1.1, readline, sqlite, xz, python@3.9, glib, gmp, bdw-gc, m4, libtool, libunistring, pkg-config, guile, libidn2, libtasn1, nettle, p11-kit, libevent, c-ares, jemalloc, libev, nghttp2, unbound, gnutls, jpeg, libpng, libslirp, libssh, libusb, lzo, ncurses, pixman, snappy, vde and qemu
...
zsh completions have been installed to:
  /usr/local/share/zsh/site-functions
==> Summary
🍺  /usr/local/Cellar/podman/3.3.1: 170 files, 39MB
==> Caveats
==> podman
zsh completions have been installed to:
  /usr/local/share/zsh/site-functions
```

해당 포스트 업로드 시각 기준으로 podman 3.3.1 버전이 설치됩니다.

## 초기 설정

Podman에서 관리하는 qemu로 Podman 클라이언트를 연결합니다.

```bash
podman machine init
```

아래처럼 fedora 가상머신 이미지를 내려받게 됩니다.

```
Downloading VM image: fedora-coreos-34.20210821.1.1-qemu.x86_64.qcow2.xz: done  
Extracting compressed file
```

준비가 되었으면 클라이언트와 연결합니다.

```bash
podman machine start
```

아래처럼 짧은 시간에 소켓을 연결하게 됩니다.

```
INFO[0000] waiting for clients...                       
INFO[0000] listening tcp://0.0.0.0:7777                 
...
Waiting for VM ...
```

podman 명령을 수행했을 때 아래처럼 출력되면 `podman machine start` 다시 수행합니다.

```
Error: failed to create sshClient: Connection to bastion host (ssh://core@localhost:50235/run/user/1000/podman/podman.sock) failed.: dial tcp [::1]:50235: connect: connection refused
```

## 설치 확인

podman이 정상적으로 설치되었는지 확인하려면 정보를 봅니다.

```bash
podman info
```

## 이미지 PULL

지금까지 podman 설정이 끝났으며, 공식 httpd 이미지를 검색해봅니다.

```bash
podman search httpd --filter=is-official
```

아래처럼 docker.io에 호스팅되는 httpd 이미지가 나옵니다.

```
INDEX       NAME                     DESCRIPTION                     STARS       OFFICIAL    AUTOMATED
docker.io   docker.io/library/httpd  The Apache HTTP Server Project  3660        [OK]    
```

docker hub와 fedoraproject.org 등 다양한 registry를 지원하므로 명시하는 게 좋습니다.

```bash
podman pull docker.io/library/httpd
```

아래와 같이 httpd 이미지를 로컬에 내려받습니다.

```
Trying to pull docker.io/library/httpd:latest...
Getting image source signatures
...
Writing manifest to image destination
Storing signatures
```

## 이미지 확인 및 실행

내려받은 이미지들을 확인합니다.

```bash
podman images
```

아래와 같이 이미지 정보가 있습니다.

```
REPOSITORY               TAG         IMAGE ID      CREATED     SIZE
docker.io/library/httpd  latest      f34528d8e714  4 days ago  142 MB
```

해당 httpd 이미지를 특정 포트로 백그라운드 실행힙니다.

```bash
podman run -dt -p 8080:80 docker.io/library/httpd 
```

## mac 환경에서 네트워크 이슈가 생길 때

여기서 만약 특정 포트에 접근이 안되는 경우에는 containers.conf 설정 파일을 건드려야 합니다.

```bash
vi ~/.config/containers/containers.conf 
```

`[containers]` 색션에서 `rootless_networking = "cni"` 구문을 넣어줍니다.

해당 임시 해결법은 다음 릴리즈에서 해결된다고 관련 깃허브 이슈에서 언급되었으므로, 시간이 지나고 자연스럽게 해결될 수 있습니다.

이제 다시 8080 포트로 접근해봅니다.

http://0.0.0.0:8080


## 컨테이너 확인

컨테이너 목록을 확인합니다.

```bash
podman ps -a
```

httpd 이미지로 컨테이너가 실행 중이라는 사실을 알 수 있습니다.

```
CONTAINER ID  IMAGE                           COMMAND           CREATED        STATUS            PORTS                 NAMES
571b00c14af2  docker.io/library/httpd:latest  httpd-foreground  2 minutes ago  Up 2 minutes ago  0.0.0.0:8080->80/tcp  suspicious_greider
```

## 로그 확인

로그를 확인합니다.

```bash
podman logs 571b00c14af2
```

컨테이너 (httpd)를 확인해보면, 이전에 접속했던 엑세스 로그를 볼 수 있습니다.

```
[07/Sep/2021:12:51:01 +0000] "GET / HTTP/1.1" 200 45
```

## 정리

지금까지 테스트한 다음, 열린 컨테이너를 중단하고 지웁니다.

```bash
podman stop 571b00c14af2
```

실행 중인 컨테이너 (httpd)를 중단합니다.

```bash
podman rm 571b00c14af2
```

컨테이너 (httpd)를 제거합니다.

```
podman rmi docker.io/library/httpd
```

마지막으로 내려받은 이미지도 지웁니다.

아래와 같이 httpd 이미지가 삭제됩니다.

```
Untagged: docker.io/library/httpd:latest
Deleted: f34528d8e714f1b877711deafec0d957394a86987cbba54d924bc0a6e517a7ac
```

## 마치며

```bash
alias docker=podman
```

공식 페이지에서 위 명령어를 언급할 정도로 도커와 매우 호환성이 높으며, 최근 REST API로 docker-compose도 같이 사용할 수 있습니다.

비록 윈도우와 맥에서는 클라이언트를 지원하므로 직접적인 REST API 지원은 제한되어 있지만, SSH로 리눅스의 podman과 통신해서 사용할 수 있습니다.

오픈소스 프로젝트로서 점차 발전되기를 기대합니다.
