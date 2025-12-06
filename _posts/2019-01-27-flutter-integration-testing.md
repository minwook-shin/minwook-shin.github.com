---
layout: post
title: flutter로 통합 테스트 환경 구축하기
---

오늘은 flutter에서 통합 테스트 환경을 구축하고 숫자를 카운팅하는 앱을 테스트해보려 합니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱으로 테스트할 장치를 준비해야 합니다.

준비했으면, 미리 기기 또는 에뮬레이터를 ide에 연결해줍니다.

## pubspec.yaml 작성하기

```yaml
dependencies:
  flutter:
    sdk: flutter
  test:

dev_dependencies:
  flutter_test:
    sdk: flutter
  flutter_driver:
    sdk: flutter
```

test, flutter_test 패키지를 pubspec 파일의 의존성에 추가해주고, 통합 테스트를 위한 flutter_driver도 마찬가지로 추가해줍니다.

## main.dart 앱 코드 작성하기

테스트할 앱 코드를 작성합니다.

```dart
import 'package:flutter/material.dart';
```

material 패키지를 import해줍니다.

```dart
void main() {
  runApp(MaterialApp(
    title: 'test code', 
    home: MyApp()));
}
```

처음 앱이 실행될 때 앱의 타이틀과 메인스크린 위젯을 출력합니다.

```dart
class MyApp extends StatefulWidget {
  @override
  _MyHomePageState createState() => _MyHomePageState();
}
```

이제 StatefulWidget으로 버튼, 슬라이더, 체크 박스와 같이 사용자와 상호작용하여 위젯이 변화하는 것을 묶어줍니다.

createState 메소드를 오버라이딩하여 MyAppState 객체를 생성해줍니다.

```dart
class _MyHomePageState extends State<MyApp> {
  int _counter = 0;
```

정수를 카운팅해줄 pravite한 변수를 만들어줍니다.

```dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: <Widget>[
          Text(_counter.toString(), key: Key('counter')),
        ],
      ),
```

_counter를 표시할 text 생성자를 만들어줍니다.

통합 테스트할 때에 필요한 Key를 만들어주고 값을 부여합니다.
이 key를 이용하여 해당 Text에 존재해야 할 값을 테스트할 수 있습니다.

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

Floating 버튼을 생성하고 버튼을 눌렀을 때에 _counter 변수를 증가시키게 해줍니다.

setState로 인해 값이 증가되고 화면을 그리게 됩니다.

Floating 버튼은 tooltip으로 통합 테스트에서 판별하게 됩니다.

## 드라이버 확장 코드 작성하기

```dart
import 'package:flutter_driver/driver_extension.dart';
import 'package:integration_testing/main.dart' as app;
```

driver_extension을 import하고, 앞서 작성한 메인 소스코드를 import 해줍니다.

```dart
void main() {
  enableFlutterDriverExtension();
  app.main();
}
```

확장을 활성화시키고, 앱의 메인 소스코드를 불러옵니다.



## 테스트 코드 작성하기

```dart
import 'package:flutter_driver/flutter_driver.dart';
import 'package:test/test.dart';
```

flutter_driver와 테스트 패키지를 불러옵니다.

```dart
void main() {
  group('App test case', () {
    FlutterDriver driver;
```

메인 함수에서 사용할 driver를 선언해줍니다.

```dart
    setUpAll(() async {
      driver = await FlutterDriver.connect();
    });
```

setUpAll 비동기 함수로 인해 테스트가 시작되기 전에 드라이버와 연결하게 됩니다.

```dart
    test('starts at 0', () async {
      expect(await driver.getText(find.byValueKey('counter')), "0");
    });

    test('increments', () async {
      await driver.tap(find.byTooltip('Increment'));
      expect(await driver.getText(find.byValueKey('counter')), "1");
    });
```

getText 메소드로 특정 키 값을 가진 텍스트를 찾아서 부여된 문자열을 테스트합니다.

또한 버튼에 툴팁이 있다면, 이를 바탕으로 해당 버튼을 눌렀을 때의 값을 테스트할 수도 있습니다.

```dart
    tearDownAll(() async {
      if (driver != null) {
        driver.close();
      }
    });
  });
}
```

모든 테스트가 완료된 뒤에는 드라이버를 종료하게 됩니다.

## 테스트 진행하기

```bash
flutter drive --target=[드라이버 확장 코드 위치]
```

테스트 코드를 작성했다면 프로젝트 폴더의 터미널에서 드라이버 확장 코드를 지정하여 실행합니다.

```bash
flutter drive --target=test/app.dart
```

이 예시와 같은 경우에는 test 폴더에 app이란 파일로 만들었으므로, 위와 같이 입력하고 진행합니다.

```
00:00 +0: App test case (setUpAll)
[info ] FlutterDriver: Connecting to Flutter application at http://127.0.0.1/
[trace] FlutterDriver: Isolate found with number: xxxxxxxxxx
[trace] FlutterDriver: Isolate is paused at start.
[trace] FlutterDriver: Attempting to resume isolate
[trace] FlutterDriver: Waiting for service extension
[info ] FlutterDriver: Connected to Flutter application.
00:00 +0: App test case starts at 0
00:00 +1: App test case increments
00:01 +2: App test case (tearDownAll)
00:01 +2: All tests passed!
Stopping application instance.
```

setUpAll이 먼저 실행되고, 드라이버가 연결되어 통합 테스트를 순서대로 수행하게 됩니다.

모든 테스트가 마무리되면 자동으로 종료됩니다.