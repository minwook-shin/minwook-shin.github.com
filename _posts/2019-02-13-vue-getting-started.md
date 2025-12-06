---
layout: post
title: Vue.js 설치하고 간단히 알아보기
---

오늘은 사용자 인터페이스를 만드는 view-layer에 초점을 맞춘 Vue.js 자바스크립트 라이브러리를 node로 설치해보고, ide를 설정해보려 합니다.

## nodejs 설치

우선 vue cli 3를 npm으로 전역 설치하기 위해서는 node가 필요합니다.

```
https://nodejs.org/ko/download/
```

윈도우의 경우에는 위 공식 사이트에서 내려받을 수 있습니다.

```
brew install node
```

맥의 경우에는 homebrew로 설치할 수 있습니다.

```
curl -sL https://deb.nodesource.com/setup_11.x | sudo -E bash -
sudo apt-get install -y nodejs
```

리눅스의 경우에는 curl을 이용하여 11버전의 노드를 설치할 수 있습니다.

## Vue CLI 설치

Vue 프로젝트를 쉽게 구성할 수 있는 Vue CLI 3을 사용하기 위해서 아래와 같이 터미널에 작성합니다.

```
npm install -g @vue/cli
```

global 옵션을 부여해서 전역으로 설치합니다.

## vscode 확장 설치

비주얼 스튜디오 코드에서 Vue VS Code Extension Pack을 설치하여 lint와 같은 확장 도구가 준비되도록 합니다.

## 구글 크롬 확장 설치

크롬에서 Vue.js devtools 확장을 설치하고 크롬의 개발자 도구를 열면 자동으로 vue 프로젝트의 서버를 인식하고 컴포넌트를 디버깅할 수 있습니다.

추가로 Vuex도 지원합니다.

## Vue?

Vue는 mvvm 패턴의 view-model 레이어에 해당되는 view 라이브러리로서 컴포넌트들의 집합체 형태를 띄고 있습니다.

그리고 html dom의 동작을 최소화한 가상 dom 렌더링도 지원합니다.

## Vue CLI 프로젝트 생성

vue cli이 잘 설치되었는지 확인하려면 새로운 프로젝트를 만들어봅니다.

vew cli 3은 이전 버전과 다르게 create 명령어로 작동합니다.

```
vue create hello-world
```

디폴트 세팅 값인 babel과 eslint만 적용하겠으면 바로 엔터키를 누르고, 아니면 manually로 방향키를 움직여서 선택해줍니다.

잠시 기다리면 기본적인 프로젝트 세팅부터 node install, Git hooks까지 완료됩니다.

## 웹으로 관리하기

만약 gui로 프로젝트를 만들고 대시보드로 관리하고 싶다면 터미널에 아래와 같이 입력해줍니다.

```
vue ui
```

웹 브라우저가 실행되면서 vue 프로젝트를 생성하고, 프리셋을 지정하는 과정을 진행할 수 있습니다.

## 개발 서버 구동하기

```
npm run serve
```

개발 서버가 구동되면서 웹팩 서버가 시작됩니다.

만약 서버를 구동한 채로 소스 파일이 수정되면 자동으로 개발 서버에 반영됩니다.

## 배포하기

```
npm run build
```

웹팩으로 배포를 위한 빌드를 진행하게 됩니다.