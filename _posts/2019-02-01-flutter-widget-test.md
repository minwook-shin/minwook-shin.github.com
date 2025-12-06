---
layout: post
title: flutter로 간단한 Widget Test 해보기
---

오늘은 flutter에서 제공하는 위젯 테스트로 기본 예제 앱을 테스트해보려 합니다.

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
class _MyHomePageState extends State<MyHomePage> {
  int _counter = 0;
```

_counter 정수형 private 변수를 선언해서 0으로 초기화합니다.

```dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Text(_counter.toString()),
      ),
```

Text 생성자로 인하여 _counter 정수형 변수를 문자열로 변환한 뒤에 출력해줍니다.

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

## 위젯 테스트 코드 작성하기

VScode 혹은 인텔리J ide에서 새로운 프로젝트를 만들었으면 위젯 테스트 코드도 기본 예제인 카운팅 앱에 맞게 작성되어 있습니다.

```dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';
import 'package:widget_test/main.dart';
```

테스트 코드에서 머티리얼 아이콘을 사용하기 위한 material 패키지와 테스트를 위한 flutter_test 패키지를 가져옵니다.

```dart
void main() {
  testWidgets('Counter increments smoke test', (WidgetTester tester) async {
    await tester.pumpWidget(MyApp());
```

pumpWidget 메소드로 MyApp 객체를 이용하여 위젯을 그려줍니다.

```dart
    expect(find.text('0'), findsOneWidget);
    expect(find.text('1'), findsNothing);
```

특정 텍스트로 되어있는 위젯을 찾는 테스트할 수 있습니다.

처음 상태는 정수형 변수를 "0"으로 출력하기 때문에 findsOneWidget으로 한 위젯을 찾을 수 있고, 값은 아직 증가하지 않았으므로 "1"의 텍스트로 이루어진 위젯은 findsNothing으로 찾을 수 없습니다.

```dart
    await tester.tap(find.byIcon(Icons.add));
```

Icons.add 아이콘이 그려진 위젯을 찾아서 탭할 수 있습니다.

```dart
    await tester.pump();
```

pump 메소드로 화면을 새로 고칠 수 있습니다.

```dart
    expect(find.text('0'), findsNothing);
    expect(find.text('1'), findsOneWidget);
  });
}
```

아까 작성했던 특정 텍스트로 되어있는 위젯을 다시 테스트해줄 수 있습니다.

버튼을 한번 클릭했기 때문에 텍스트가 "1"로 되어있므로 이 위젯은 findsOneWidget으로 하나의 위젯을 찾을 수 있습니다.


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