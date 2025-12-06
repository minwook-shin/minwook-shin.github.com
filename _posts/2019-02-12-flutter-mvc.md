---
layout: post
title: flutter에서 mvc 적용하기
---

오늘은 flutter 프로젝트에서 mvc 구조로 만드는 방법을 작성하려 합니다.

다른 프로그래밍 언어에서도 자주 사용되는 mvc 구조는 모델과 뷰, 그리고 컨트롤러로 구분지어서 인터페이스와 로직을 분리시키는 디자인 패턴 중 하나입니다.

## pubspec.yaml 작성하기

```yaml
dependencies:
  flutter:
    sdk: flutter
  mvc_pattern:
```

flutter로 작성된 앱의 구조를 mvc로 만들기 위해 mvc_pattern 패키지를 추가해줍니다.

## main.dart 앱 코드 작성하기

이제 앱 코드를 보겠습니다.

```dart
import 'package:flutter/material.dart';
import 'package:mvc_pattern/mvc_pattern.dart';
```

머티리얼 디자인 앱을 만들기 위한 material 패키지와 mvc 구조의 디자인 패턴으로 앱를 구현하기 위한 mvc_pattern 패키지를 가져옵니다.

```dart
class Model {
  static int _counter = 0;
  static int _increment() => ++_counter;
  static int _decrement() => --_counter;
}
```

mvc 구조에서의 모델을 구현한 클래스입니다.

변수와 함수가 묶어져서 동작 코드만 작성되있는 모습입니다.

```dart
class Controller extends ControllerMVC {
  static get counter => Model._counter;
  static void incrementController() => Model._increment();
  static void decrementController() => Model._decrement();
}
```

mvc 구조에서의 컨트롤러를 ControllerMVC로 확장해서 구현한 클래스입니다.

사용자는 모델을 직접 건들이지 않고 컨트롤러로 조작하게 됩니다.

```dart
void main() => runApp(MyApp());
```

AppMVC로 확장한 MyApp 객체를 메인함수에서 앱으로 실행합니다.

```dart
class MyApp extends AppMVC {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter mvc',
      home: View(),
    );
  }
}
```

StatefulWidget의 View 객체를 머티리얼 디자인으로 감싸주고 화면을 그립니다.

```dart
class View extends StatefulWidget {
  @override
  State createState() => ViewState();
}
```

StatefulWidget으로 버튼, 슬라이더, 체크 박스처럼 사용자와 상호작용하여 위젯이 변화하는 것을 묶어줍니다.

createState 메소드를 오버라이딩하여 ViewState 객체를 생성해줍니다.


```dart
class ViewState extends StateMVC<View> {
  ViewState() : super(Controller());
```

StateMVC를 확장하여 Controller를 전달해줍니다.

```dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Text(Controller.counter.toString()),
```

Text 생성자를 이용하여 컨트롤러에서 counter 변수를 문자열로 가져와 출력합니다.

위 코드에서는 Center와 mainAxisAlignment의 조합으로 가운데 정렬이 된 채로 출력됩니다.

```dart
            OutlineButton(
                onPressed: () {
                  setState(() {
                    Controller.incrementController();
                  });
                },
                child: Icon(Icons.add)),
```

버튼을 만들고, onPressed 콜백에 setState를 수행하게 합니다.

버튼이 눌리면 Controller에서 increment 함수를 호출하여 값이 증가하고, setState에 의해 화면이 다시 그려지게 됩니다.

```dart
            OutlineButton(
                onPressed: () {
                  setState(() {
                    Controller.decrementController();
                  });
                },
                child: Icon(Icons.remove))
          ],
        ),
      ),
    );
  }
}
```

버튼을 하나 더 만들고, onPressed 콜백에 setState를 수행하게 합니다.

버튼이 눌리면 Controller에서 decrement 함수를 호출하여 값이 감소하고, setState에 의해 화면이 다시 그려지게 됩니다.