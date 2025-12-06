---
layout: post
title: flutter의 mockito로 mock 객체를 생성해서 유닛 테스트 해보기
---

오늘은 flutter에서 지원하는 mockito를 이용하여 mock 객체로 유닛 테스트해보려 합니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱으로 테스트할 장치를 준비해야 합니다.

준비했으면, 미리 기기 또는 에뮬레이터를 ide에 연결해줍니다.

## pubspec.yaml 작성하기

```yaml
dependencies:
  flutter:
    sdk: flutter
  mockito:

dev_dependencies:
  flutter_test:
    sdk: flutter
```

유닛 테스트를 위한 패키지와 mockito 패키지를 pubspec 파일의 의존성에 추가해줍니다.

## 테스트를 위한 클래스 작성하기

지난 유닛 테스트에서 사용한 Calc 클래스를 보강하고, Helper 클래스를 이용하여 랜덤으로 값을 추가로 올려보려 합니다.

하지만, Helper 클래스를 mock 객체로 만들어서 Calc 클래스의 기능을 중심으로 테스트해보려 합니다.

```dart
class Calc {
  int value = 0;

  void increment() => value+=1;
```

별도의 파일에 값을 증가시키는 Calc 클래스를 만들고, increment 메소드로 인해 값이 증가됩니다.

```dart
  var help= Helper();
  int syncHelper(help) {
    this.value = help.setup();
    this.value = help.bonusClick();
    increment();
    return this.value;
  }
}
```

Helper 클래스와 연결되는 함수를 만들어서 아직 미구현된 setup과 bonusClick를 사용해줍니다.

increment로 인해 Helper 객체에서 가져온 값에서 증가하게 됩니다.

```dart
class Helper{
  int setup() => 0;
  int bonusClick() => 0;
}
```

Helper 클래스는 아직 구현되지 않았습니다.

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
    title: 'mock test', 
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

## mockito 테스트 코드 작성하기

```dart
import 'package:mockito/mockito.dart';
import 'package:flutter_test/flutter_test.dart';
import '../lib/calc.dart';
import 'package:mockito_basic/helper.dart';
```

mock 객체로 테스트를 진행하기 위해 mockito 패키지와 flutter_test를 가져옵니다.

```dart
class MockHelper extends Mock implements Helper {}
```

mock 클래스를 구성해줍니다.

```dart
void main() {
  group('mock', () {
    test('verify', () {
      var helper = MockHelper();
      helper.bonusClick();
      verify(helper.bonusClick());
    });
```

helper를 Mock으로 생성한 다음에 verify로 어떤 메소드가 수행되었는지 검증할 수 있습니다.

```dart
    test('verify call', () {
      var helper = MockHelper();
      helper.bonusClick();
      helper.bonusClick();
      verify(helper.bonusClick()).called(2);
    });
```

helper를 Mock으로 생성한 다음에 verify로 어떤 메소드가 몇번 수행되었는지 검증할 수 있습니다.

```dart
    test('bonusClick return', () {
      var helper = MockHelper();
      var calc = Calc();
      when(helper.bonusClick()).thenReturn(10);
      expect(calc.syncHelper(helper), 11);
    });
```

구현된 Calc 객체에 helper를 Mock으로 생성한 객체를 사용하여 bonusClick 메소드에 10이 반환된다는 가정하에 calc의 syncHelper 메소드가 11이 되는지 테스트할 수 있습니다.

Calc 클래스 코드에서 increment 메소드를 한번 더 수행했으므로 11이 반환됩니다.

```dart
    test('reset', () {
      var helper = MockHelper();
      var calc = Calc();
      when(helper.bonusClick()).thenReturn(10);
      expect(calc.syncHelper(helper), 11);
      reset(helper);
      expect(helper.bonusClick(), null);
    });
  });
}
```

mock을 초기화해줍니다.

## mock 객체로 테스트 진행하기

```bash
flutter test
```

해당 프로젝트의 모든 테스트를 실행하고 싶다면 위와 같이 입력합니다.

```bash
flutter test test/calc_test.dart
```

이와 같은 경우에는 mockito가 사용된 test 폴더의 calc_test 파일만 테스트하게 합니다.

모든 테스트가 마무리되면 자동으로 종료됩니다.