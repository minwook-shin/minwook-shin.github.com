---
layout: post
title: flutter로 텍스트 폼 포커스 사용해보기
---

오늘은 flutter로 텍스트 필드의 포커스를 원할 때에 맞출 수 있는 앱을 만들어보려 합니다.

이 포스팅을 따라 오신다면 여러분도 텍스트 필드의 포커스를 내 맘대로 하는 앱을 안드로이드, ios 앱으로 동시에 만들어 볼 수 있습니다.

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
    title: 'Form Focus',
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
  FocusNode focusNode1;
  FocusNode focusNode2;
```

State 클래스로 FocusNode의 라이프사이클을 관리해줍니다.

그리고 포커스 트리에 있는 텍스트 필드를 식별하기 위해 사용되는 FocusNode를 텍스트 필드 갯수대로 선언해줍니다.

```dart
  @override
  void initState() {
    super.initState();
    focusNode1 = FocusNode();
    focusNode2 = FocusNode();
  }

  @override
  void dispose() {
    super.dispose();
    focusNode1.dispose();
    focusNode2.dispose();
  }
```

initState 메소드로 FocusNode를 만들고, dispose 메소드로는 FocusNode를 정리하게 합니다.

```dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Column(
        children: [
          TextField(
            autofocus: true,
            focusNode: focusNode1,
          ),
          TextField(
            focusNode: focusNode2,
          ),
```

텍스트 필드를 두 개 만들어서 각각의 폼에 서로 다른 FocusNode를 전달해줍니다.

그리고 autofocus를 true로 했을 경우에는 앱이 실행될 때에 자동으로 포커싱이 됩니다.

```dart
          OutlineButton(
            onPressed: () => FocusScope.of(context).requestFocus(focusNode1),
            child: Text("첫번째 텍스트로 포커스 이동"),
          ),
          OutlineButton(
            onPressed: () => FocusScope.of(context).requestFocus(focusNode2),
            child: Text("두번째 텍스트로 포커스 이동"),
          ),
        ],
      ),
    );
  }
}
```

버튼을 텍스트 필드의 밑에 배치하고, 버튼을 눌렀을 때에 각각의 텍스트 필드에 포커싱이 되도록 매칭시킵니다.

requestFocus 메소드로 텍스트 필드에 전달해준 FocusNode를 사용해주면 해당 텍스트 필드로 포커싱이 됩니다.