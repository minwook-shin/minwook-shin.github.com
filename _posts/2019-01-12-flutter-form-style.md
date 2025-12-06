---
layout: post
title: flutter로 텍스트 폼 스타일 만들어보기
---

오늘은 flutter로 텍스트 필드의 스타일을 바꾸어 본 앱을 만들어보려 합니다.

이 포스팅을 따라 오신다면 여러분도 텍스트 필드의 스타일을 내 맘대로 하는 앱을 안드로이드, ios 앱으로 동시에 만들어 볼 수 있습니다.

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
void main() => runApp(MaterialApp(
      title: 'Form style',
      home: Scaffold(
        body: MyApp(),
      ),
    ));

```

처음 앱이 실행될 때 앱의 타이틀과 메인스크린 위젯을 출력합니다.

```dart
class MyApp extends StatefulWidget {
  @override
  MyAppState createState() => MyAppState();
}
```

이제 StatefulWidget으로 버튼, 슬라이더, 체크 박스와 같이 사용자와 상호작용하여 위젯이 변화하는 것을 묶어줍니다.

createState 메소드를 오버라이딩하여 _MyAppState 객체를 생성해줍니다.

```dart
class MyAppState extends State<MyApp> {
  final _key = GlobalKey<FormState>();
```

위젯 트리에서 부모로 접근해서 자식에게 전달할 수 있는 글로벌 키를 만들어서 텍스트 필드의 유효성을 검사할 때 사용해줍니다.

언더바를 사용하면 해당 변수는 private합니다.

```dart
  @override
  Widget build(BuildContext context) {
    return Form(
      key: _key,
```

만든 글로벌 키를 폼에 대입하여 폼을 식별할 수 있고, 유효성 검사를 할 수 있게 도와줍니다.

```dart
      child: Column(
        children: [
          TextFormField(
            validator: (value) {
              if (value.isEmpty) {
                return '텍스트 필드에 작성된 문자열이 없습니다.';
              }
            },
```

TextFormField에는 validator로 값이 비어있다면 문자열을 출력하도록 만들어줍니다.

```dart
            decoration: InputDecoration(
                labelText: 'Enter your username',
                icon: Icon(Icons.account_box)),
            maxLength: 5,
            initialValue: "초기 값",
            style: TextStyle(color: Colors.blue),
            autofocus: true,
            onEditingComplete: () {
              _key.currentState.validate();
            },
          ),
```

어제 작성한 텍스트 필드를 유효성 검사하는 코드와 다른 점은 decoration 속성과 같이 양식을 꾸밀 수 있다는 것입니다.

아이콘과 라벨 텍스트를 생성하거나, 최대 입력 길이를 제한하고, 색상을 미리 지정해두는 작업들은 해둘 수 있습니다.

특히 타 언어에서는 이벤트 리스너라고 불리는 것들이 속성에 익명 함수 형태로 넣을 수 있습니다.

```dart
          OutlineButton(
            onPressed: () {
              _key.currentState.validate();
            },
            child: Text("버튼"),
          ),
        ],
      ),
    );
  }
}
```

버튼을 누르면 양식이 유효한지 판단하고, 값이 비어있다면 메시지를 출력하게 됩니다.