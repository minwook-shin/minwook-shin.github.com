---
layout: post
title: flutter를 맥에 설치하고 앱 빌드하기
---

오늘은 flutter 1.0 stable이 나온지 얼마 안되서 한번 체험해보고자 실습하면서 포스팅을 작성했습니다.

포스팅에서는 mac을 기준으로 설명하지만, 윈도우나 리눅스도 sdk받고 실행해서 doctor로 의존성 검사하는 과정은 비슷합니다.

다만, 윈도우와 리눅스는 안드로이드 스튜디오로 안드로이드 앱만 빌드할 수 있습니다.

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
$ vim $HOME/.bash_profile
```

만약 zsh를 사용하고 있다면 $HOME/.zshrc에서 작성합니다.

```bash
$ vim $HOME/.zshrc
```

아래 코드를 .bash_profile(혹은 .zshrc)의 하단에 입력합니다.

```
export PATH="$HOME/flutter/bin:$PATH"
```

## flutter 업그레이드하기

```bash
$ flutter upgrade
```

만약 flutter 버전을 업그레이드하고 싶으시다면, 터미널에서 위와 같이 작성하세요.

## flutter doctor 실행하기

```bash
$ flutter doctor
```

설치를 완료하기 위해서는 의존성을 확인하는 작업을 해야 합니다.

처음 수행하면 다트 언어에 대한 sdk가 자동으로 설치되며, flutter 도구를 빌드합니다.

flutter doctor를 수행해서 부족한 점이 발견되면 missing 경고를 띄우며 설치 방식을 안내합니다.

부족한 의존성을 설치하고, 다시 flutter doctor를 수행해서 확인해야 합니다.

만약 익명으로 전송되는 보고를 끄려면 

```bash
$ flutter config --no-analytics
```

위와 같이 입력합니다.

## 플랫폼 설치

맥은 Flutter로 안드로이드, ios앱을 만들 수 있으므로 두 플랫폼을 설치하고 설정합니다.

## iOS 설정하기

Xcode를 설치하시고, 아래와 같이 터미널에 입력합니다.

```bash
$ sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
```

Xcode를 처음 설치했다면 라이선스 동의를 해야 할 수도 있습니다.

```bash
$ sudo xcodebuild -license
```

## iOS 시뮬레이터 설정하기

```bash
$ open -a Simulator
$ flutter run
```

시뮬레이터로 빌드하려면 위와 같이 작성하고 프로젝트 폴더에서 run해주면 됩니다.

## iOS 기기 배포하기

ios 기기에서 테스트하기 위해 배포를 하려면 사전설치가 필요합니다.

```bash
$ brew update

$ brew install --HEAD usbmuxd
$ brew link usbmuxd
$ brew install --HEAD libimobiledevice
$ brew install ideviceinstaller ios-deploy cocoapods
$ pod setup
```

brew로 라이브러리를 설치해주고, brew doctor를 수행합니다.

```bash
$ brew doctor
```

이후에 타 ios 빌드처럼 프로젝트 signing을 진행하면 배포를 할 수 있습니다.

```
$ flutter run
```

시뮬레이터와 마찬가지로 run 해줍니다.

## Android 설정하기

안드로이드 기기 혹은 가상머신으로 앱을 배포하려면 안드로이드 스튜디오(가상 기기 포함)를 설치하면 됩니다.

안드로이드 스튜디오에서 flutter 플러그인을 설치해줍니다.

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