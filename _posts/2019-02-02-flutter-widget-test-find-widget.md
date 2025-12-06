---
layout: post
title: flutter의 Widget Test에서 위젯 찾아보기
---

오늘은 flutter에서 제공하는 위젯 테스트로 테스트를 할 때에 위젯을 찾을 수 있는 방법을 알아보려 합니다.

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

메인 코드도 VScode 혹은 인텔리J ide에서 새로운 프로젝트를 만들었으면 생성되는 기본 예제 앱에서 조금 수정하여 사용해보려 합니다.

```dart
import 'package:flutter/material.dart';
```

material 패키지를 가져옵니다.

```dart
void main() => runApp(MyApp());
```

이번 위젯 테스트를 위한 MyApp 객체를 생성하고, runApp으로 앱을 실행합니다. 

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo', 
      home: MyHomePage()
      );
  }
}
```

MyApp은 MaterialApp으로 감싼 제목과 MyHomePage 객체입니다.

```dart
class MyHomePage extends StatefulWidget {
  @override
  _MyHomePageState createState() => _MyHomePageState();
}
```

이제 StatefulWidget으로 버튼, 슬라이더, 체크 박스와 같이 사용자와 상호작용하여 위젯이 변화하는 것을 묶어줍니다.

createState 메소드를 오버라이딩하여 _MyHomePageState 객체를 생성해줍니다.

```dart
  int _counter = 0;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Text('$_counter'),
```

private한 정수형 변수 _counter를 Text 생성자에 의하여 화면에 출력됩니다.

```dart
            OutlineButton(
              child: Icon(Icons.publish),
              onPressed: () {
                setState(() {
                  _counter++;
                });
              },
            ),
```

OutlineButton을 만들어서 특정 Icon을 넣습니다.

이 버튼을 누르게 되면 _counter 값이 증가하고 setState에 의해 화면이 새로고쳐집니다.

```dart
            OutlineButton(
              key: Key("download"),
              child: Icon(Icons.file_download),
              onPressed: () {
                setState(() {
                  _counter--;
                });
              },),
          ],
        ),
      ),
```

OutlineButton을 만들어서 특정 Icon을 넣습니다.

이 버튼을 누르게 되면 _counter 값이 감소하고 setState에 의해 화면이 새로고쳐집니다.

```dart
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          setState(() {
            _counter++;
          });
        },
        tooltip: 'Increment',
        child: Icon(Icons.add),
      ),
    );
  }
}
```

floating 버튼을 만들어서 눌러주었을 때에 _counter 변수의 값을 증가시킵니다.

tooltip으로 인하여 위젯 테스트를 할 때 위젯을 찾을 수 있습니다.

## 위젯 테스트 코드 작성하기

메인 코드 작성이 끝나면 위젯 테스트를 할 때에 어떻게 위젯을 찾는지 알아보려 합니다.

```dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';
import 'package:finding_widget/main.dart';
```

우선 material 디자인의 아이콘으로 찾기 위해서 material 패키지를 가져오고, 위젯 테스트를 위해서 flutter_test 패키지도 가져옵니다.

```dart
void main() {
  testWidgets('text', (WidgetTester tester) async {
    await tester.pumpWidget(MyApp());
    expect(find.text('0'), findsOneWidget);
  });
```

pumpWidget으로 메인 코드의 MyApp 객체를 화면에 그려주고, 텍스트가 '0'인 위젯을 찾습니다.

```dart
  testWidgets('byIcon', (WidgetTester tester) async {
    await tester.pumpWidget(MyApp());
    expect(find.byIcon(Icons.publish), findsOneWidget);
  });
```

pumpWidget으로 메인 코드의 MyApp 객체를 화면에 그려주고, 아이콘이 '0'인 위젯을 찾습니다.

```dart
  testWidgets('byTooltip', (WidgetTester tester) async {
    await tester.pumpWidget(MyApp());
    expect(find.byTooltip('Increment'), findsOneWidget);
  });
```

pumpWidget으로 메인 코드의 MyApp 객체를 화면에 그려주고, 툴팁이 'Increment'인 위젯을 찾습니다.

```dart
  testWidgets('byKey', (WidgetTester tester) async {
    await tester.pumpWidget(MyApp());
    expect(find.byKey(Key("download")), findsOneWidget);
  });
```

pumpWidget으로 메인 코드의 MyApp 객체를 화면에 그려주고, 키값이 "download"인 위젯을 찾습니다.

```dart
  testWidgets('byType', (WidgetTester tester) async {
    await tester.pumpWidget(MyApp());
    expect(find.byType(FloatingActionButton), findsOneWidget);
  });
```

pumpWidget으로 메인 코드의 MyApp 객체를 화면에 그려주고, 위젯 타입이 FloatingActionButton인 위젯을 찾습니다.

```dart
  testWidgets('byWidget', (WidgetTester tester) async {
    expect(find.byWidget(Padding(padding: EdgeInsets.zero)), findsNothing);
  });
}
```

특정 위젯 객체를 찾고 싶으면, byWidget을 사용합니다.

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