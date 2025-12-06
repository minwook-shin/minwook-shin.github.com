---
layout: post
title: flutter로 위젯 페이드 인 아웃하는 앱 만들어보기
---

오늘은 flutter로 버튼을 눌렀을 때에 위젯 애니메이션으로 페이드 인하고 아웃하는 앱을 만들어보려 합니다.

이 포스팅을 따라 오신다면 여러분도 위젯을 페이드 인하고 아웃하는 애니메이션 기능을 할 수 있는 안드로이드, ios 앱을 동시에 만들어 볼 수 있습니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱을 구동할 장치를 준비해야 합니다.

준비했으면, 미리 기기를 ide에 연결해줍니다.

## pubspec.yaml 작성하기

이번 포스팅에서는 따로 설정할 것이 없으므로 new project로 설정된 그대로 사용합니다.

## main.dart 작성하기

이제 메인 코드를 작성해보아야 합니다.

```dart
import 'package:flutter/material.dart';
```

flutter/material 패키지를 사용하겠다는 것이지만, 대부분의 앱에서 반드시 가져와야 하는 필수 패키지입니다.

```dart
void main() {
  runApp(MaterialApp(
    title: 'Returning Data',
    home: MyApp(),
  ));
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

언더바를 사용하면 해당 클래스는 private합니다.

```dart
class _MyAppState extends State<MyApp> {
  bool _visible = true;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: ListView(
        children: [
          AnimatedOpacity(
            opacity: _visible ? 1.0 : 0.1,
            child: Container(
              height: 500,
              color: Colors.blue,
            ),
          ),
```

State 객체에서는 변수 값을 저장하는 것처럼 위젯의 상태는 객체에 따로 저장되어 관리되므로, 클래스도 기존의 위젯과 따로 만들어줍니다.

_visible을 삼항연산자로 판별하여 투명도를 조절합니다.

1에 가까울수록 불투명해지고, 0에 가까울수록 투명합니다.

```dart
          OutlineButton(
            child: Text("fade in/out"),
            onPressed: () {
              setState(() {
                _visible = !_visible;
              });
            },
          )
        ],
      ),
    );
  }
}
```

버튼을 누르면 true는 false가 되고, false는 true가 되며 setState 메소드를 불러옵니다.

AnimatedOpacity의 opacity 속성에 존재하는 삼항연산자로 인해 투명도가 조절됩니다.