---
layout: post
title: 우분투를 리셋하고 싶다면 resetter를 이용해보세요
---

오늘은 우분투를 공장 초기화하고 싶으신 분을 위한 오픈소스 프로그램을 소개해드려 합니다.

## 개요

프로그램 이름은 Resetter이며, 깃허브 아이디가 gaining이신 개발자분께서 개발했습니다.

## 오픈소스

오픈소스 라이선스는 GNU General Public License v3.0입니다.

## 용도

프로젝트의 readme 파일에서 보이다시피 설치 이미지로 수동으로 설치할 필요없이 우분투 및 기타 배포판을 재설정, 즉 초기화하는데 도움이 되는 프로그램이라고 적혀있습니다.

## 개발 언어

프로그램은 현재 python2로 작성되어있으며, 깃허브 이슈를 보면 언젠가 pyqt5와 python3에 이식 할 계획이 있다고 합니다.

## 지원되는 배포판

* Debian 9.2
* Linux Mint 17.3+
* Ubuntu 14.04+
* Elementary OS 0.4+
* Linux Deepin 15.4+    

## 설치 방법

내려받은 deb 파일을 터미널에서 찾은 다음 

```bash
sudo apt install gdebi
sudo gdebi add-apt-key_1.0-0.5_all.deb
sudo gdebi resetter_2.2.0-stable_all.deb
``` 

위 명령어를 입력합니다.


## 초기화 옵션 

Automatic Reset과 Custom Reset이 존재합니다.

Automatic Reset은 앱들을 자동으로 삭제하여 초기화하고, 사용자 계정과 홈 디렉토리를 자동으로 삭제해줍니다.

그리고 Custom Reset은 지울 앱과 사용자 계정을 선택할 수 있으며, 오래된 커널도 지울 수 있습니다.

## 기타 기능 

Easy install이라고 해서 설치할 앱 목록을 한번에 설치할 수 있게 도와주는 기능이 있으며,  Easy PPA 또한, ppa를 쉽게 추가시켜줍니다.

그리고 Source Editor는 ppa를 검색하며 편집할 수 있다고 합니다.

## resetter 깃허브 주소

https://github.com/gaining/Resetter 에서 내려받거나 이슈와 pull request를 보낼 수 있습니다.

## 주의점

제가 직접 초기화해보면서 체감한 것이지만, 영어 외의 언어의 우분투로 작업하면 영어로 돌아갑니다.

이 글을 보는 대부분은 주로 한국어로 설정되있을태니, resetter 프로그램으로 초기화할 때는 주의를 요구합니다.