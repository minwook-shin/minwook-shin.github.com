---
layout: post
title: Flutter로 Dart DevTools 사용해보기
---

오늘은 Flutter 1.2 버전이 발표되면서 소개된 "New web-based tooling for developers building Flutter applications."인 Dart DevTools를 알아보고 직접 해보려 합니다.

## Dart DevTools

Dart와 Flutter에서 사용하는 성능 도구 제품입니다.

2019년 2월 기준으로 미리보기 버전입니다.

Flutter 앱의 UI 레이아웃 및 상태를 검사해주거나, Flutter 앱의 UI jank 성능 문제를 진단해줍니다.

디버깅 및 일반 로그 및 진단 정보도 지원합니다.

## flutter 업그레이드

```
flutter upgrade
```

Upgrading Flutter from \*...라고 출력되면서 git 정보를 업데이트하고, Upgrading engine...이라고 출력되면서 Dart SDK을 포함하여 flutter 엔진을 업데이트합니다.

마지막으로 Running flutter doctor...라면서 flutter doctor 명령어가 자동으로 수행되고 종료됩니다.

```
flutter --version
```

업그레이드를 끝내고 위 명령어로 flutter 버전을 확인해보면

```
Flutter 1.2.1 • channel stable • https://github.com/flutter/flutter.git
```

stable이라도 1.2 이상의 버전으로 올라간 것을 확인할 수 있습니다.

## 설치

안드로이드 스튜디오랑 vs code 그리고 터미널에서 사용할 수 있습니다

## 안드로이드 스튜디오

안드로이드 스튜디오나 인텔리j에서는 Flutter 플러그인을 설치하면 사용할 수 있습니다.

안드로이드 스튜디오에서 DevTools toolbar를 사용하거나, 맥 기준 (cmd-shift-a) 단축키로 Dart DevTools를 열 수 있습니다.

## vs code

vscode에서도 flutter 확장을 설치해야 합니다.

디버그 매뉴에서 디버깅 시작 (F5)을 눌러 디버그를 활성화하고, 명령 팔레트에서 Dart:Open DevTools를 타이핑하여 사용할 수 있습니다.

우측 하단에서 활성화하라고 출력되면 버튼을 클릭하여 DevTools 패키지를 활성화해줍니다.

Dart DevTools가 활성화되는 동안 vs code의 상태 표시줄에 나오게 되며, 자유롭게 웹 브라우저로 확인할 수 있습니다.

## 터미널

만약 터미널에서 작업한다면,

```
flutter packages pub global activate devtools
```

위 명령어를 수행하여 devtools를 설치합니다.

```
flutter packages pub global run devtools
```

위 명령어를 수행하여 로컬 웹 서버를 시작합니다.

```
flutter run --observatory-port=9200
```

```
http://localhost:9100/?port=9200
```

애뮬레이터나 실 기기가 연결된 상태로 flutter 프로젝트 폴더에서 위 명령어를 수행하고, 위 주소로 접근하면 됩니다.

## 기능

- Flutter Inspector

Flutter 위젯 트리를 시각화하고 탐색 할 수 있습니다.

- Timeline View

프레임 렌더링에 대한 정보를 표시합니다.

- Logging View

어플리케이션 레벨의 로그를 표시 합니다.

```dart
import 'dart:io';
import 'dart:developer' as developer;
```

```dart
void _incrementCounter() {
    setState(() {
        stderr.writeln(_counter.toString() + ' clicked');
        developer.log(_counter.toString() + ' clicked', name: 'my.app');
        _counter++;
    });
  }
```

io 패키지의 stderr로 로그를 남기거나, developer 패키지의 log로 로그를 라벨을 붙여 남길 수있습니다.

- Debugger

디버깅을 지원하지만, 이미 에디터에서 디버그를 지원하는 경우에는 Debugger는 비활성화 상태입니다.
