---
layout: post
title: flutter로 텍스트 폼 내용 받기
---

오늘은 flutter의 TextEditingController를 이용하여 텍스트 필드에 작성한 텍스트를 가져오는 앱을 만들어보려 합니다.

이 포스팅을 따라 오신다면 여러분도 텍스트 필드에 작성한 글들을 쉽게 가져와서 처리하는 앱을 안드로이드, ios 앱으로 동시에 만들어 볼 수 있습니다.

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
    title: 'Retrieve Text',
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

```dart
class _MyAppState extends State<MyApp> {
  TextEditingController _textController;
```

State 클래스로 TextEditingController의 라이프사이클을 관리해줍니다.

```dart
  @override
  void initState() {
    super.initState();
    _textController = TextEditingController();
  }

  @override
  void dispose() {
    super.dispose();
    _textController.dispose();
  }
```

initState 메소드로 TextEditingController를 만들고, dispose 메소드로는 TextEditingController를 정리하게 합니다.

TextEditingController를 생성해주고, 원하는 텍스트 필드에 컨트롤러로 건내주면 해당 텍스트 필드에서 작성한 문자열을 가져올 수 있게 됩니다.

```dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
        body: Column(
      children: <Widget>[
        TextField(
          controller: _textController,
        ),
```

텍스트 필드를 가져와서 controller로 이전에 만든 textController 변수를 건내줍니다.

```dart
        OutlineButton(
          onPressed: () {
            print(_textController.text);
          },
        )
      ],
    ));
  }
}
```

이제 버튼을 만들어서 콘솔창에 텍스트 필드의 문자열을 출력하도록 작성해봅니다.