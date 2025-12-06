---
layout: post
title: flutter 컨텐츠 공유 기능 사용하기
---

오늘은 flutter로 컨텐츠를 공유하는 기능인 share 패키지에 대하여 알아보려 합니다.

안드로이드에서는 ACTION_SEND 인텐트이며, ios에서는 UIActivityViewController라고 불립니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱으로 테스트할 장치를 준비해야 합니다.

준비했으면, 미리 기기 또는 에뮬레이터를 ide에 연결해줍니다.

## pubspec.yaml 작성하기

```yaml
dependencies:
  flutter:
    sdk: flutter
  share:
```

flutter 앱에 포함된 컨텐츠를 공유하는 기능을 위하여 share 패키지를 pubspec.yaml에 작성해줍니다.

## main.dart 앱 코드 작성하기

```dart
import 'package:flutter/material.dart';
import 'package:share/share.dart';
```

앱에 머티리얼 위젯을 추가할 수 있는 material 패키지와 컨텐츠를 공유할 share 패키지를 가져옵니다.

```dart
void main() => runApp(MyApp());
```

MyApp 객체를 앱으로 구동합니다.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter share',
      home: MyHomePage(),
    );
  }
}
```

머티리얼 디자인으로 감싸진 MyHomePage 객체를 화면에 그려줍니다.

```dart
class MyHomePage extends StatefulWidget {
  @override
  _MyHomePageState createState() => _MyHomePageState();
}
```

StatefulWidget으로 버튼, 슬라이더, 체크 박스처럼 사용자와 상호작용하여 위젯이 변화하는 것을 묶어줍니다.

createState 메소드를 오버라이딩하여 \_MyHomePageState 객체를 생성해줍니다.

```dart
class _MyHomePageState extends State<MyHomePage> {
  String text = '';
```

문자열 변수를 선언하고 초기화합니다.

```dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            TextField(
              onChanged: (String value) => setState(() {
                    text = value;
                  }),
            ),
```

TextField를 만들어주고, onChanged 콜백에 의해 내용이 바뀌면 text 문자열 변수에 값을 넣어줍니다.

이 작업을 진행할 때에는 setState에 의해 화면이 새로 고쳐집니다.

```dart
            OutlineButton(
              child: Icon(Icons.share),
              onPressed: text.isEmpty
                  ? null
                  : () {
                      Share.share(text);
                    },
            )
          ],
        ),
      ),
    );
  }
}
```

onPressed 콜백에 의하여 문자열이 존재할 때만 컨텐츠를 공유하게 됩니다.

안드로이드에서 ACTION_SEND 인텐트, ios에서는 UIActivityViewController로 구현한 것처럼 출력되게 됩니다.
