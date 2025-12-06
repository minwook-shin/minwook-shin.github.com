---
layout: post
title: flutter로 Shared Preferences 구현하기
---

오늘은 flutter에서 Shared Preferences를 이용하여 임시로 데이터를 읽고 쓰는 앱을 만들어보려 합니다.

이 포스팅을 따라 오신다면 여러분도 키 값을 이용하여 간단한 데이터를 쓰고 읽는 앱을 안드로이드, ios 앱으로 동시에 만들어 볼 수 있습니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱을 구동할 장치를 준비해야 합니다.

준비했으면, 미리 기기를 ide에 연결해줍니다.

## pubspec.yaml 작성하기

shared_preferences 외부 패키지를 pubspec 파일의 의존성에 추가해줍니다.

```yaml
dependencies:
  flutter:
    sdk: flutter
  shared_preferences:
```

SharedPreferences와 관현된 메소드를 사용할 수 있습니다.

## main.dart 작성하기

이제 메인 코드를 작성해보아야 합니다.

```dart
import 'package:flutter/material.dart';
import 'package:shared_preferences/shared_preferences.dart';
```

material 패키지와 shared_preferences 외부 패키지를 사용하기 위해서 가져와줍니다.

```dart
void main() {
  runApp(MaterialApp(
        title: 'Shared preferences', 
        home: Scaffold(body: MyApp())
        ));
}
```

처음 앱이 실행될 때 앱의 타이틀과 메인스크린 위젯을 출력합니다.

```dart
class MyApp extends StatefulWidget {
  @override
  MyAppState createState() => MyAppState();
}
```

이제 StatefulWidget으로 버튼, 슬라이더, 체크 박스와 같이 사용자와 상호작용하여 위젯이 변화하는 것을 묶어줍니다.

createState 메소드를 오버라이딩하여 MyAppState 객체를 생성해줍니다.

```dart
class MyAppState extends State<MyApp> {
  int _counter;
```

_counter 변수로 숫자가 카운팅되는 것을 세줍니다.

변수 앞에 밑줄이 있음으로서 이 변수가 private하다고 선언해줍니다.

```dart
  @override
  void initState() {
    super.initState();
    (() async {
      SharedPreferences prefs = await SharedPreferences.getInstance();
      setState(() {
        _counter = prefs.getInt('counter');
      });
    })();
  }
```

initState 메소드를 오버라이딩하여 앱이 구동될 때에 수행할 내용을 정의해줍니다.

익명 함수를 비동기로 생성해주고, getInstance 메소드로 SharedPreferences 객체를 가져와줍니다.

SharedPreferences 객체로 미리 지정한 키 값을 기입하면 해당되는 키 값과 매치된 데이터를 불러와줍니다.

값을 불러올 때에는 setState 메소드에 의해 화면이 다시 그려집니다.

```dart
  @override
  Widget build(BuildContext context) {
    return ListView(
      children: [
        Text(_counter.toString(), textAlign: TextAlign.center),
```

ListView에 Text 생성자를 작성하고, _counter 변수의 값을 문자열의 형태로 출력하게 합니다.

```dart
        OutlineButton(
            onPressed: () async {
              SharedPreferences prefs = await SharedPreferences.getInstance();
              setState(() {
                _counter++;
                prefs.setInt('counter', _counter);
              });
            },
            child: Icon(Icons.add)),
```

ListView에 버튼을 만들어줍니다.

버튼의 onPressed 콜백으로 인하여 _counter 변수로 증가시킨 값을 SharedPreferences 객체에 설정해줍니다.

```dart
        OutlineButton(
            onPressed: () async {
              SharedPreferences prefs = await SharedPreferences.getInstance();
              setState(() {
                _counter--;
                prefs.setInt('counter', _counter);
              });
            },
            child: Icon(Icons.remove)),
      ],
    );
  }
}
```

ListView에 버튼을 더 만들어줍니다.

버튼의 onPressed 콜백으로 인하여 _counter 변수로 감소시킨 값을 SharedPreferences 객체에 설정해줍니다.