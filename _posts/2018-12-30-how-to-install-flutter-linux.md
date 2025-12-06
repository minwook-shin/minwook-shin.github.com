---
layout: post
title: flutter를 리눅스에 설치하고 앱 빌드하기
---

오늘은 지난번에 작성한 "flutter를 맥에 설치하는 포스팅"을 리눅스에서 설치하는 방법으로 다시 작성해보았습니다.

이 포스팅은 맥에서 설치하는 과정과 매우 비슷하나, 설치해야 되는 의존성과 필요없는 ios 빌드 설정을 생략하였습니다.

ios 앱을 빌드하려면 이전에 작성한 "맥에 설치하기" 포스팅을 보시면 됩니다.

## flutter란

리엑트 네이티브와 같이 안드로이드, ios 앱을 한번에 빌드해주는 모바일 프레임워크입니다.

리엑트 네이티브와 달리 자바스크립트 브릿지를 쓰지않고, 다트 언어라는 네이티브 코드로 작성되어 더 빠르게 코딩할 수 있으며 핫 리로드로 빠르게 앱을 수정할 수 있습니다.

## Flutter SDK 가져오기

이 글을 쓰는 시점에서 최신 버전인 Flutter SDK 1.0 를 내려받습니다.

https://storage.googleapis.com/flutter_infra/releases/stable/macos/flutter_macos_v1.0.0-stable.zip

혹은 깃허브에서 stable 상태의 최신 버전을 가져오는 방법도 있습니다.

```bash
$ git clone -b stable https://github.com/flutter/flutter.git
```

flutter tool에 대한 경로를 추가합니다.

```bash
$ export PATH=$PATH:`pwd`/flutter/bin
```

이는 현재 터미널에서만 적용되므로 경로를 업데이트해야 합니다.

경로를 계속 유지하려면 $HOME/.bash_profile에서 작성합니다.

```bash
$ vim .bash_profile
```

만약 zsh를 사용하고 있다면 $HOME/.zshrc에서 작성합니다.

```bash
$ vim .zshrc
```

아래 코드를 .bash_profile(혹은 .zshrc) 파일의 하단에 입력합니다.

```
$ export PATH=$PATH:flutter/bin
```

위 코드를 입력하고 .bash_profile(혹은 .zshrc)를 저장했다면 아래 명령어를 수행해줍니다.

```bash
$ source $HOME/.bash_profile
```

## flutter 업그레이드하기

```bash
$ flutter upgrade
```

만약 flutter 버전을 업그레이드하고 싶으시다면, 터미널에서 위와 같이 작성하세요.

## flutter doctor 실행하기

일단 flutter doctor를 수행하기 전에 

```
bash, mkdir, rm, git, curl, unzip, which, xz-utils
```

위 도구들이 작동해야 합니다.

보통은 git과 curl만 설치해주면 대부분 모든 도구가 있게됩니다.

```dart
$ sudo apt install git curl
```

```bash
$ flutter doctor
```

설치를 완료하기 위해서는 의존성을 확인하는 작업을 해야 합니다.

처음 수행하면 다트 언어에 대한 sdk가 자동으로 설치되며, flutter 도구를 빌드합니다.

flutter doctor를 수행해서 부족한 점이 발견되면 missing 경고를 띄우며 설치 방식을 안내합니다.

리눅스에서는 lib32stdc++6 의존성을 따로 설치해야 할 수도 있습니다.

```bash
$ sudo apt install lib32stdc++6 
``` 

부족한 의존성을 설치하고, 다시 flutter doctor를 수행해서 확인해야 합니다.

만약 익명으로 전송되는 보고를 끄려면 

```bash
$ flutter config --no-analytics
```

위와 같이 입력합니다.

## 플랫폼 설치

리눅스는 Flutter로 안드로이드앱만 만들 수 있으므로 안드로이드 스튜디오를 설치하고 설정합니다.
빌드하려면 위와 같이 작성하고 프로젝트 폴더에서 run해주면 됩니다.

## Android 설정하기

안드로이드 기기 혹은 가상머신으로 앱을 배포하려면 안드로이드 스튜디오(가상 기기 포함)를 설치하면 됩니다.

안드로이드 스튜디오에서 flutter 플러그인을 설치해줍니다.

안드로이드 라이선스에 동의하는 작업을 해야 할 수도 있습니다.

## 안드로이드 기기 설정하기

기존에 개발하던 방식처럼 기기의 개발자 옵션에서 usb 디버깅을 활성화하고, 기기와 pc를 연결합니다.

```
$ flutter devices 
```

터미널에서 flutter와 기기를 연결하는 작업을 수행합니다.

## Android 애뮬레이터 설정하기

가상화 가속을 지원하는 pc에서 안드로이드 스튜디오 AVD Manager로 가상 기기를 생성합니다.

## vscode 설치하고 설정하기

비주얼스튜디오 코드를 설치하고 Flutter와 Dart 플러그인을 설치합니다.

Flutter Doctor로 의존성을 다시 확인해줍니다.

## 앱 테스트하기

비주얼스튜디오 코드에서 명령 팔레트로 flutter: new preject를 선택하고, 처음에 sdk 위치를 지정한 다음 새로운 프로젝트 이름을 정하면 자동으로 hello_world/lib/main.dart 가 생성됩니다.

비주얼 스튜디오의 우측 하단의 no device를 누르고 원하는 에뮬레이터를 선택하면 자동으로 그 기기가 실행되면서 앱이 빌드됩니다.

## 핫 리로드

실행중인 어플리케이션의 상태는 그대로 두고, 일부만 빌드해서 앱을 빠르게 수정할 수 있습니다.

핫 리로드를 하려면 "r"를 누르고, 재빌드를 하려면 "R"을 누르면 됩니다.