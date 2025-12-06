---
layout: post
title: flutter quick_actions 사용하여 빠른 액션 추가하기
---

오늘은 flutter로 ios의 Home Screen Quick Actions과 안드로이드의 App shortcuts을 제공해주는 quick_actions 패키지에 대하여 알아보려 합니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱으로 테스트할 장치를 준비해야 합니다.

준비했으면, 미리 기기 또는 에뮬레이터를 ide에 연결해줍니다.

## pubspec.yaml 작성하기

```yaml
dependencies:
  flutter:
    sdk: flutter
  quick_actions:
```

flutter 앱에 빠른 액션을 추가하기 위하여 quick_actions 패키지를 pubspec.yaml에 작성해줍니다.

## 이전 버전

```
It is safe to run this plugin
with earlier versions of Android as it will produce a noop.
```

해당 패키지의 설명에 나온 것처럼 이전 버전의 안드로이드에서는 신경 쓸 필요가 없다고 합니다.

이전 버전이라고 함은 API level 25 이하의 버전을 가리킵니다.

## main.dart 앱 코드 작성하기

```dart
import 'package:flutter/material.dart';
import 'package:quick_actions/quick_actions.dart';
```

앱에 머티리얼 위젯을 추가할 수 있는 material 패키지와 빠른 액션을 추가해줄 quick_actions 패키지를 가져옵니다.

```dart
void main() => runApp(MyApp());
```

앱을 MyApp으로 구동합니다.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter quick_actions',
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
  @override
  void initState() {
    super.initState();
    final QuickActions quickActions = QuickActions();
    quickActions.initialize((String shortcutType) {
      if (shortcutType == 'action_main') {
        print("main");
      }
      if (shortcutType == 'action_help') {
        print("help");
      }
    });
```

initState을 오버라이딩하여 앱이 실행될 때 시작될 작업을 정의해줍니다.

QuickActions의 객체를 생성히고 quickActions 콜백을 생성합니다.

빠른 액션을 정의해둔대로 shortcutType을 분리하여 각각의 액션을 눌렀을 때 수행할 작업을 작성해둡니다.

```dart
    quickActions.setShortcutItems(<ShortcutItem>[
      ShortcutItem(
          type: 'action_main', localizedTitle: '메인 스크린', icon: 'icon_main'),
      ShortcutItem(
          type: 'action_help', localizedTitle: '도움말', icon: 'icon_help')
    ]);
  }
```

타입은 shortcutType으로 구별할 수 있어야 하므로 고유한 이름이여야 하며, localizedTitle으로 사용자에게 보이는 이름을 정의할 수 있습니다.

아이콘은 ios의 xcassets 혹은 안드로이드의 drawable의 이름을 따라갑니다.

```dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Text(""),
      ),
    );
  }
}
```

빈 문자열을 포함한 Text 생성자로 화면이 그려지게 되면서 앱 자체는 아무것도 존재하지 않고, 오직 앱 서랍에서만 해당 앱 아이콘을 길게 눌렀을 때에 빠른 액션들이 나타나게 됩니다.
