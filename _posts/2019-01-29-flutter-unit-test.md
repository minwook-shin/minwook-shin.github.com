---
layout: post
title: flutter로 유닛 테스트 환경 구축하기
---

오늘은 flutter에서 유닛 테스트 환경을 구축하여 기본 예제 앱에 적용하여 테스트해보려 합니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱으로 테스트할 장치를 준비해야 합니다.

준비했으면, 미리 기기 또는 에뮬레이터를 ide에 연결해줍니다.

## pubspec.yaml 작성하기

```yaml
dev_dependencies:
  test:
  flutter_test:
    sdk: flutter
```

유닛 테스트를 위해서 test, flutter_test 패키지를 pubspec 파일의 의존성에 추가해줍니다.

## 테스트를 위한 클래스 작성하기

```dart
class Calc {
  int value = 0;
  void increment() => value++;
}
```

별도의 파일에 값을 증가시키는 Calc 클래스를 만들고, increment 메소드로 인해 값이 증가되게 작성하였습니다.

## main.dart 앱 코드 작성하기

위 Calc 클래스를 사용하여 기본 예제 앱 코드를 수정해줍니다.

```dart
import 'package:flutter/material.dart';
import 'calc.dart';
```

material 패키지와 위에서 작성한 Calc 클래스 파일을 가져옵니다.

```dart
void main() {
  runApp(MaterialApp(
    title: 'unit test', 
    home: MyApp()));
}
```

처음 앱이 실행될 때 앱의 타이틀과 메인스크린 위젯을 출력합니다.

```dart
class MyApp extends StatefulWidget {
  @override
  _MyAppState createState() => _MyAppState();
}
```

이제 StatefulWidget으로 버튼, 슬라이더, 체크 박스와 같이 사용자와 상호작용하여 위젯이 변화하는 것을 묶어줍니다.

createState 메소드를 오버라이딩하여 _MyAppState 객체를 생성해줍니다.

```dart
class _MyAppState extends State<MyApp> {
  final calc = Calc();
```

Calc 객체를 만듭니다.

```dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Text(calc.value.toString()),
      ),
```

Calc 객체의 value라는 정수형 필드를 문자열로 변환한 뒤에 출력합니다.

```dart
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          setState(() {
            calc.increment();
          });
        },
        tooltip: 'tooltip',
        child: Icon(Icons.add),
      ),
    );
  }
}
```

floating 버튼을 선언해주고, Calc 객체에 존재하는 값을 증가시킬 메소드를 불러옵니다.

setState 메소드에 의해서 화면이 다시 그려지게 되면서 값이 증가된 것이 출력됩니다.

이렇게 코드를 작성해서 앱을 빌드하면 기본 예제 앱처럼 버튼을 누르자마자 값이 증가됩니다.

## 유닛 테스트 코드 작성하기

```dart
import 'package:test/test.dart';
import 'package:unit_test/calc.dart';
```

유닛 테스트를 위해서 test 패키지와 값을 증가시키는 기능을 가진 calc 클래스 파일을 가져옵니다.

```dart
void main() {
  group('Adder function', () {
    test('start at 0', () {
      expect(Calc().value, 0);
    });
```

처음에 value 기댓값이 0인지 테스트합니다.

```dart
    test('incremented', () {
      final calc = Calc();
      calc.increment();
      expect(calc.value, 1);
    });
  });
}
```

increment 메소드를 실행했을 때에 기댓값이 증가하는지 테스트해줍니다.

## 테스트 진행하기

```bash
flutter test
```

해당 프로젝트의 모든 테스트를 실행하고 싶다면 위와 같이 입력합니다.

```bash
flutter test test/calc_test.dart
```

이와 같은 경우에는 test 폴더의 calc_test 파일만 테스트하게 합니다.

모든 테스트가 마무리되면 자동으로 종료됩니다.