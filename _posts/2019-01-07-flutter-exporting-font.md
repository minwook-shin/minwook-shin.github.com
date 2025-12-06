---
layout: post
title: flutter에서 커스텀 폰트 사용하기
---

오늘은 커스텀 폰트 파일을 이용하여 flutter로 앱을 만들어보려 합니다.

이 포스팅을 따라 오신다면 여러분도 여러 폰트를 사용하는  안드로이드, ios 앱을 동시에 만들어 볼 수 있습니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱을 구동할 장치를 준비해야 합니다.

준비했으면, 미리 기기를 ide에 연결해줍니다.

## pubspec.yaml 작성하기

폰트의 위치를 지정하고 폰트의 스타일이 노말인지, 이탤릭체인지 정할 수 있습니다.

```yaml
flutter:
  fonts:
    - family: Nanum Gothic
      fonts:
        - asset: lib/fonts/NanumGothic_Regular.ttf
    - family: Nanum Gothic bold
      fonts:
        - asset: lib/fonts/NanumGothic-Bold.ttf
  uses-material-design: true
```

이 포스트에서는 구글 폰트 사이트에서 받은 나눔 고딕을 사용했으며, 폰트 패밀리를 지정하여 텍스트별로 글꼴을 지정할 수 있게 하였습니다.

## main.dart 작성하기

이제 메인 코드를 작성해보아야 합니다.

```dart
import 'package:flutter/material.dart';
```

flutter/material 패키지를 사용하겠다는 것이지만, 대부분의 앱에서 반드시 가져와야 하는 필수 패키지입니다.

```dart
void main() => runApp(MaterialApp(
      title: 'Fonts',
      home: MyApp(),
    ));
```

처음 앱이 실행될 때 앱의 타이틀과 메인스크린 위젯을 출력합니다.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
        body: ListView(
      children: <Widget>[
        Text(
          '기본입니다.',
        ),
```

기본적인 문자열은 Text로 출력할 수 있습니다.

```dart
        Text(
          '구글 폰트 사이트에서 받은 나눔 고딕입니다.',
          style: TextStyle(
            fontFamily: 'Nanum Gothic',
            fontStyle: FontStyle.normal,
          ),
        ),
```

만약 글꼴을 다르게하고 싶다면, 이전에 작성한 pubspec 파일에서 지정한 폰트 패밀리를 기입해야 합니다.

```dart
        Text(
          '구글 폰트 사이트에서 받은 나눔 고딕 볼드체입니다.',
          style: TextStyle(
            fontFamily: 'Nanum Gothic bold',
            fontStyle: FontStyle.normal,
          ),
        ),
      ],
    ));
  }
}
```

볼드체는 따로 지정했으므로 별개의 폰트로 처리됩니다.