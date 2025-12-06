---
layout: post
title: flutter의 Widget Test에서 텍스트 입력하기
---

오늘은 flutter에서 제공하는 위젯 테스트로 텍스트 필드에 문자열을 넣어 테스트하는 방법을 알아보려 합니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱으로 테스트할 장치를 준비해야 합니다.

준비했으면, 미리 기기 또는 에뮬레이터를 ide에 연결해줍니다.

## pubspec.yaml 작성하기

```yaml
dependencies:
  flutter:
    sdk: flutter

dev_dependencies:
  flutter_test:
    sdk: flutter
```

위젯 테스트를 위해 flutter_test 패키지를 pubspec 파일의 의존성에 추가해줍니다.

VScode 혹은 인텔리J ide에서 새로운 프로젝트를 만들었으면 기본적으로 pubspec에 작성되어 있습니다.

## main.dart 앱 코드 작성하기

메인 코드를 작성해야 합니다.

```dart
import 'package:flutter/material.dart';
```

머티리얼 디자인의 앱을 만들기 위해서 material 패키지를 가져옵니다.

```dart
void main() => runApp(MyApp());
```

MyApp 객체를 앱으로 구동합니다.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter entering text',
      home: MyHomePage(),
    );
  }
}
```

머티리얼 디자인으로 감싸진 MyAppStateful 객체를 화면에 그려줍니다.

```dart
class MyHomePage extends StatefulWidget {
  @override
  _MyHomePageState createState() => _MyHomePageState();
}
```

이제 StatefulWidget으로 버튼, 슬라이더, 체크 박스와 같이 사용자와 상호작용하여 위젯이 변화하는 것을 묶어줍니다.

createState 메소드를 오버라이딩하여 _MyHomePageState 객체를 생성해줍니다.

```dart
class _MyHomePageState extends State<MyHomePage> {
  String _str = "";
  final controller = TextEditingController();
```

private한 문자열 변수를 선언해주고, 텍스트 필드에서 문자열을 가져오기 위해서 컨트롤러도 생성해줍니다.

```dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            TextField(
              controller: controller,
              key: Key('textForm'),
            ),
```

텍스트 필드에 컨트롤러를 부여하고, 테스트를 위해 키에도 키 값을 부여해줍니다.

```dart
            OutlineButton(
                onPressed: () {
                  setState(() {
                    _str = controller.text.toString();
                  });
                },
                child: Text("data"),
                key: Key('button')),
```

버튼을 만들어서 눌렀을 때에 컨트롤러가 부여된 텍스트 필드의 값을 가져와서 문자열 변수에 담아줍니다.

setState에 의해 화면이 다시 그려집니다.

테스트할 때에 버튼이 필요하므로 이 역시 키 값을 부여해줍니다.

```dart
            Text(_str, key: Key('text')),
          ],
        ),
    );
  }
}
```

문자열을 출력할 Text 생성자도 만들어줍니다.

## 위젯 테스트 코드 작성하기

메인 코드 작성이 끝나면 위젯 테스트를 할 때에 어떻게 위젯을 찾는지 알아보려 합니다.

```dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';
import 'package:entering_text_widget_test/main.dart';
```

위젯의 키를 사용하기 위한 material 패키지와 위젯 테스트를 위한 flutter_test 패키지를 가져옵니다.

```dart
void main() {
  testWidgets('entering test', (WidgetTester tester) async {
    await tester.pumpWidget(MyApp());
```

pumpWidget으로 메인 코드의 MyApp 객체를 화면에 그려줍니다.

```dart
    await tester.enterText(find.byKey(Key('textForm')), "hello");
```

enterText로 'textForm'으로 기록되있는 키를 저장하고 있는 위젯을 찾아서 특정 문자열을 입력시킵니다.

```dart
    await tester.tap(find.byKey(Key('button')));
```

tap으로 특정 키 값을 가진 버튼을 눌러줍니다.

```dart
    await tester.pump();
```

pump 메소드로 화면을 새로 고쳐줍니다.

```dart
    expect(find.byKey(Key('text')), findsOneWidget);
  });
}
```

'text'라는 키 값이 저장된 text 위젯이 존재하는지 테스트할 수 있습니다.

## 테스트 진행하기

```bash
flutter test
```

해당 프로젝트의 모든 테스트를 실행하고 싶다면 위와 같이 입력합니다.

```bash
flutter test test/widget_test.dart
```

이와 같은 경우에는 test 폴더의 widget_test 파일만 테스트하게 합니다.

모든 테스트가 마무리되면 자동으로 종료됩니다.