---
layout: post
title: flutter 통합 테스트로 스크롤 앱 테스트하기
---

오늘은 flutter에서 통합 테스트 환경으로 리스트 스크롤 앱을 테스트해보려 합니다.

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
    title: 'List test code', 
    home: MyApp()));
}
```

처음 앱이 실행될 때 앱의 타이틀과 메인스크린 위젯을 출력합니다.

```dart
class MyApp extends StatelessWidget {
  final List<String> items = List<String>.generate(100, (i) => i.toString());
```

List.generate로 items라는 리스트를 만들어줍니다.

```dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: ListView.builder(
        key: Key('listview'),
        itemCount: items.length,
```

앱에 ListView를 배치하고, 테스트를 위해 ListView에 키를 부여합니다.

```dart
        itemBuilder: (context, index) {
          return ListTile(
            onTap: () {
              print("click test");
            },
            title: Text(items[index].toString(), key: Key(index.toString())));
        },
      ),
    );
  }
}
```

각 타일에도 키를 부여하여 해당 키값이 들은 위젯이 화면에 존재하는가에 대하여 테스트할 수 있습니다.

## 드라이버 확장 코드 작성하기

```dart
import 'package:flutter_driver/driver_extension.dart';
import 'package:scrolling_test/main.dart' as app;
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
  group('Scrolling test code', () {
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
    test('verifies item', () async {
      await driver.scrollUntilVisible(
          find.byValueKey('listview'), find.byValueKey('30'),
          dyScroll: -100);
      expect(await driver.getText(find.byValueKey('30')), '30');
    });
```

driver.scrollUntilVisible로 인해 위젯이 보이는 순간까지 스크롤합니다.

find.byValueKey로 ListView와 각각의 리스트 타일도 찾을 수 있습니다.

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
00:00 +0: Scrolling test code (setUpAll)
[info ] FlutterDriver: Connecting to Flutter application at http://127.0.0.1/
[trace] FlutterDriver: Isolate found with number: xxxxxxxxxx
[trace] FlutterDriver: Isolate is paused at start.
[trace] FlutterDriver: Attempting to resume isolate
[trace] FlutterDriver: Waiting for service extension
[info ] FlutterDriver: Connected to Flutter application.
00:00 +0: Scrolling test code verifies item
00:08 +1: Scrolling test code (tearDownAll)
00:08 +1: All tests passed!
Stopping application instance.
```

setUpAll이 먼저 실행되고, 드라이버가 연결되어 통합 테스트를 순서대로 수행하게 됩니다.

모든 테스트가 마무리되면 자동으로 종료됩니다.