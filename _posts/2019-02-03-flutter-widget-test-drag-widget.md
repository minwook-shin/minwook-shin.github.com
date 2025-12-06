---
layout: post
title: flutter의 Widget Test에서 위젯 끌어보기
---

오늘은 flutter에서 제공하는 위젯 테스트로 위젯을 드래그하여 테스트하는 방법을 알아보려 합니다.

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

머티리얼 디자인 앱을 만들기 위해 material 패키지를 가져옵니다.

```dart
void main() => runApp(MyApp());
```

MyApp 객체를 앱으로 구동합니다.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        title: 'Flutter', 
        home: Scaffold(body: MyAppStateful())
        );
  }
}
```

머티리얼 디자인으로 감싸진 MyAppStateful 객체를 화면에 그려줍니다.

```dart
class MyAppStateful extends StatefulWidget {
  @override
  _MyAppState createState() => _MyAppState();
}
```

이제 StatefulWidget으로 버튼, 슬라이더, 체크 박스와 같이 사용자와 상호작용하여 위젯이 변화하는 것을 묶어줍니다.

createState 메소드를 오버라이딩하여 _MyAppState 객체를 생성해줍니다.

```dart
class _MyAppState extends State<MyAppStateful> {
  var list = List.generate(100, (i) => i.toString());
  @override
  Widget build(BuildContext context) {
    return ListView.builder(
      itemBuilder: (context, i) {
```

0부터 99까지 인덱스가 존재하는 리스트를 List.generate로 생성해주고, ListView의 builder를 반환하면서 화면에 그려줍니다.

```dart
        return Dismissible(
            onDismissed: (d) {
              setState(() {
                list.removeAt(i);
              });
            },
```

각각의 리스트 뷰 항목에는 Dismissible이 들어갑니다.

해당 항목을 드래그하여 화면 밖으로 밀게되면, 리스트의 해당 인덱스가 제거되면서 setState에 의해 화면이 다시 그려지게 됩니다.

```dart
            child: ListTile(title: Center(child: Text(list[i].toString()))),
```

각각의 Dismissible에는 ListTile이 들어가서 문자열을 넣을 수 있습니다.

```dart
            key: Key(list[i]),
```

Key를 부여하여 위젯 테스트를 할 때에도 유용하게 테스트할 수 있습니다.

```dart
            background: Container(color: Colors.amber));
      },
```

리스트타일이 옆으로 밀릴 때 보이는 뒷 배경을 지정할 수 있습니다.

```dart
      itemCount: list.length,
    );
  }
}
```

ListView의 갯수를 지정해줍니다.

## 위젯 테스트 코드 작성하기

메인 코드 작성이 끝나면 위젯 테스트를 할 때에 어떻게 위젯을 찾는지 알아보려 합니다.

```dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';
import 'package:drag_widget_test/main.dart';
```

위젯의 키를 사용하기 위한 material 패키지와 위젯 테스트를 위한 flutter_test 패키지를 가져옵니다.

```dart
void main() {
  testWidgets('drag', (WidgetTester tester) async {
    await tester.pumpWidget(MyApp());
```

pumpWidget으로 메인 코드의 MyApp 객체를 화면에 그려줍니다.

```dart
    await tester.pump();
```

pump 메소드로 화면을 새로 고쳐줍니다.

```dart
    await tester.drag(find.byKey(Key('2')), Offset(500.0, 0.0));
```

byKey 메소드를 이용해 키값이 '2'로 부여된 위젯을 찾아서 지정한 Offset만큼 드래그합니다.

```dart
    await tester.pumpAndSettle();
```

pumpAndSettle 메소드로 계속 위젯 트리를 재갱신하여 Dismiss 효과가 끝날 때까지 화면을 새로 고쳐줄 수 있습니다.

```dart
    expect(find.text('2'), findsNothing);
    expect(find.text('1'), findsOneWidget);
  });
}
```

키 값이 '2'인 항목이 드래그되어 Dismiss되었으므로 해당 위젯이 존재하지 않았을 것이고, 키 값이 '1'인 항목은 그대로 유지되어 있을 것입니다.

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