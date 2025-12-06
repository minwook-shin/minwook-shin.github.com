---
layout: post
title: Mac 안드로이드 스튜디오 없이 Flutter 설치하기
---

오늘은 안드로이드 스튜디오 없이 Flutter 개발할 수 있는 방법을 알아보았습니다.

포스팅에서는 mac을 기준으로 설명하지만, 리눅스도 sdk받고 실행해서 doctor로 의존성 검사하는 과정은 비슷하지만, zsh 대신 bash이거나 honebrew 대신 apt로 설치할 수 있습니다.

## Flutter란

리엑트 네이티브와 같이 안드로이드, ios 앱을 한번에 빌드해주는 모바일 프레임워크입니다.

리엑트 네이티브와 달리 자바스크립트 브릿지를 쓰지않고, 다트 언어라는 네이티브 코드로 작성되어 더 빠르게 코딩할 수 있으며 핫 리로드로 빠르게 앱을 수정할 수 있습니다.

## Flutter SDK 가져오기

```
git clone -b stable https://github.com/flutter/flutter.git
```

깃허브에서 stable 상태의 최신 버전을 Flutter SDK 가져옵니다. 물론 직접 깃허브에서 Zip 파일을 받을 수 있습니다.

## Flutter 환경변수

flutter tool에 대한 경로를 정의합니다.

```
export PATH=$PATH:`pwd`/flutter/bin
```

이는 현재 터미널에서만 적용되므로 경로를 업데이트해야 합니다.

```
vi .zshrc
export PATH="$HOME/flutter/bin:$PATH"
source .zshrc
```

경로를 계속 유지하려면 $HOME/.zshrc에서 작성합니다.

## vscode Flutter

안드로이드 스튜디오 대신에 사용할 vscode에서 dart-code.flutter 확장을 설치합니다.

## Flutter doctor

```
flutter doctor
```

설치를 완료하기 위해서는 의존성을 확인하는 작업을 해야 합니다.

처음 수행하면 다트 언어에 대한 sdk가 자동으로 설치되며, flutter 도구를 빌드합니다.

flutter doctor를 수행해서 부족한 점이 발견되면 missing 경고를 띄우며 설치 방식을 안내합니다.

만약 익명으로 전송되는 보고를 끄려면

```
flutter config --no-analytics
```

위와 같이 입력합니다.

## Homebrew 사용한 android-sdk 설치

원래 안드로이드 스튜디오를 설치할 때 같이 포함되는 android-sdk를 Homebrew로 별도 설치합니다.

Android SDK 패키지를 설치하고 업데이트하거나 제거할 수 있습니다.

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"

brew cask install android-sdk
```

brew cask로 android-sdk를 설치합니다.

## ANDROID_SDK_ROOT 설정

맥에 ANDROID_SDK 경로를 업데이트합니다.

```
vi .zshrc
export ANDROID_SDK_ROOT=”/usr/local/share/android-sdk”
source .zshrc
```

경로를 계속 유지하려면 $HOME/.zshrc에서 작성합니다.

## repositories.cfg 오류 수정

```
mkdir -p .android && touch ~/.android/repositories.cfg
```

만약 repositories.cfg 관련 오류가 발생한다면, .android 폴더와 repositories.cfg 파일를 만듭니다.

## OpenJDK 설치

```
brew tap AdoptOpenJDK/openjdk
brew cask install adoptopenjdk8
```

android-sdk 실행할 수 있게 JDK를 설치합니다.

## Flutter android-sdk 경로 지정

```
flutter config --android-sdk /usr/local/share/android-sdk
```

설치된 android-sdk를 flutter에 지정합니다.

## sdk manager

```
sdkmanager --update
```

android-sdk가 잘 수행되는 지 확인하면서, 설치된 모든 android sdk 패키지 업데이트합니다.

```
sdkmanager --list
sdkmanager "platform-tools" "platforms;android-29" "build-tools;29.0.3"
flutter doctor --android-licenses
flutter doctor
```

android-sdk로 안드로이드 플랫폼 도구를 내려받고, flutter doctor로 안드로이드 라이선스를 수락합니다.

```
sdkmanager --list
sdkmanager "system-images;android-29;google_apis_playstore;x86_64"
```

vscode를 열고, Command palette로 Flutter 프로젝를 생성한 뒤에 안드로이드 에뮬레이터 디바이스를 엽니다.

만약 Cannot launch without an active device라는 오류가 출력되면 android-sdk로 android 시스템 이미지를 내려받습니다.

다시 VScode로 돌아와서 F5키를 누르면, 안드로이드 에뮬레이터 디바이스로 앱을 디버깅할 수 있습니다.