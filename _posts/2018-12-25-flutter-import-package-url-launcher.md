---
layout: post
title: flutter로 패키지 가져와서 사용해보기
---

오늘은 flutter로 외부 패키지를 사용해보려 합니다.

그 예시로 외부 브라우저에 웹 페이지를 띄어주는 url_launcher라는 패키지를 이용하여 작성해보려 합니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱을 구동할 장치를 준비해야 합니다.

준비했으면, 미리 기기를 ide에 연결해줍니다.

## pubspec.yaml 작성하기

pubspec.yaml를 작성해서 패키지의 이름이나 버전 그리고 의존성 패키지들을 관리할 수 있습니다.

https://pub.dartlang.org/flutter/ 에서 flutter 패키지를 조회하고 내려받을 수 있습니다.

이번 포스팅에서는 url_launcher 패키지를 가져옵니다.

```yaml
dependencies:
  flutter:
    sdk: flutter
  url_launcher:
```

기본적으로 새로운 프로젝트를 생성하면 dependencies에 url_launcher 패키지 의존성을 추가해줍니다.

## package get

pubspec.yaml 파일을 작성했으면 아래 packages get 명령어를 터미널에서 사용하여 패키지를 로컬에 받아줍니다.

```bash
flutter packages get
```

## main.dart 작성하기

이제 메인 코드를 작성해보아야 합니다.

```dart
import 'package:flutter/material.dart';
```

flutter/material 패키지를 사용하겠다는 것이지만, 대부분의 앱에서 반드시 가져와야 하는 필수 패키지입니다.

```dart
import 'package:url_launcher/url_launcher.dart';
```

url_launcher 패키지를 사용하기 위해서 해당 패키지를 import합니다.

```dart
void main() {
  runApp(MaterialApp(
    home: MainPage(),
  ));
}
```

앱이 실행되면 MainPage 위젯을 화면에 출력합니다.

```dart
class MainPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: OutlineButton(
        onPressed: () {
          launch('https://www.google.com');
        },
        child: Text('홈페이지 링크'),
      ),
    );
  }
}
```

OutlineButton을 누르면 url_launcher 패키지의 launch메소드에 의해서 링크가 외부 브라우저로 넘어갑니다.