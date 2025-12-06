---
layout: post
title: flutter로 안드로이드 intent 사용해보기
---

오늘은 안드로이드에서 자주 사용되는 intent를 flutter에서도 사용해보려 합니다.

해당 플러그인은 flutter 팀이 만들었으며, ios는 url_launcher 패키지를 사용하라고 권장하고 있습니다.

안드로이드에서도 url_launcher 패키지로 대체되어 사용할 수 있으나, android_intent 패키지를 사용하면 추가적으로 인자를 넣어 전송할 수 있습니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱으로 테스트할 장치를 준비해야 합니다.

준비했으면, 미리 기기 또는 에뮬레이터를 ide에 연결해줍니다.

## pubspec.yaml 작성하기

```yaml
dependencies:
  flutter:
    sdk: flutter
  android_intent:
```

안드로이드에서의 intent를 flutter에서도 구현하기 위해 android_intent 패키지를 pubspec.yaml에 작성해줍니다.

## main.dart 앱 코드 작성하기

```dart
import 'package:flutter/material.dart';
import 'package:android_intent/android_intent.dart';
import 'package:platform/platform.dart';
```

머티리얼 디자인 위젯을 불러오기 위한 material 패키지와 intent를 구현할 android_intent 패키지를 가져옵니다.

그리고 플랫폼 구별을 통해 안드로이드에서만 코드를 실행시키기 위한 platform 패키지도 가져옵니다.

```dart
void main() => runApp(MyApp());
```

main 함수를 작성하고 앱이 실행되면 MyApp가 수행되도록 합니다.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Android Intent',
      home: MyHomePage(),
    );
  }
}
```

머티리얼 디자인으로 감싸진 MyHomePage 객체를 화면에 그려줍니다.

```dart
class MyHomePage extends StatefulWidget {
  @override
  _MyHomePageState createState() => _MyHomePageState();
}
```

StatefulWidget으로 버튼, 슬라이더, 체크 박스처럼 사용자와 상호작용하여 위젯이 변화하는 것을 묶어줍니다.

createState 메소드를 오버라이딩하여 \_MyHomePageState 객체를 생성해줍니다.

```dart
class _MyHomePageState extends State<MyHomePage> {
  void createIntent() async {
    if (LocalPlatform().isAndroid) {
      final AndroidIntent intent = AndroidIntent(
          action: 'action_view',
          data: Uri.encodeFull('https://flutter.dev'),
          package: 'com.android.chrome');
      intent.launch();
    }
  }
```

구동하는 플랫폼이 안드로이드인지 판별하기 위해 LocalPlatform의 isAndroid로 조건문을 걸어주고, intent를 만들어줍니다.

intent로 launch()해주면 수행됩니다.

```dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            OutlineButton(
              child: Text("크롬으로 url 열기"),
              onPressed: createIntent,
            )
          ],
        ),
      ),
    );
  }
}
```

OutlineButton을 그려주고 해당 버튼을 눌렀을 때에 미리 만들어둔 createIntent 함수를 수행하게 합니다.
