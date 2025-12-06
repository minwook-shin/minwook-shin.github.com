---
layout: post
title: flutter showAboutDialog로 About Dialog 추가하기
---

오늘은 flutter로 About Dialog 를 보여줄 수 있는 showAboutDialog 에 대하여 알아보려 합니다.

최근 블로그 앱을 업데이트하면서 이름과 버전 그리고 라이선스 버튼 포함된 대화 상자를 구현할 일이 있었는데, Flutter에서 이미 구현되어 있다는 소식을 듣고 적용해보았습니다.

## showAboutDialog 위젯

showAboutDialog 위젯으로 앱에 대한 정보와 오픈소스 라이선스 매뉴를 쉽게 준비할 수 있습니다.

위젯이 구성하는데 필요한 주요 속성을 나열해보았습니다.

* applicationName : 애플리케이션 이름

* applicationVersion : 애플리케이션 버전

* applicationIcon : 애플리케이션 아이콘

* applicationLegalese : 애플리케이션 copyright 정보

라이선스 목록은 앱에서 사용하는 라이선스 파일을 조회하여 자동으로 생성됩니다.

showAboutDialog에 대한 자세한 내용은 [이 주의 Flutter 위젯](https://youtu.be/YFCSODyFxbE) 영상을 참고하시면 좋습니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱으로 테스트할 장치를 준비해야 합니다.

준비했으면, 미리 기기 또는 에뮬레이터를 IDE에 연결해줍니다.

## pubspec.yaml 작성하기

해당 위젯은 기본적으로 작성된 pubspec.yaml에서 사용할 수 있습니다.

## main.dart 앱 코드 작성하기

```dart
import 'package:flutter/material.dart';
```

앱에 머티리얼 위젯을 추가할 수 있는 material 패키지를 가져옵니다.

```dart
void main() => runApp(MyApp());
```

MyApp 객체를 앱으로 구동합니다.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter about dialog',
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

StatefulWidget으로 버튼처럼 사용자와 상호작용하여 위젯이 변화하는 것을 묶어줍니다.

createState 메소드를 오버라이딩하여 \_MyHomePageState 객체를 생성해줍니다.

```dart
class _MyHomePageState extends State<MyHomePage> {
```

\_MyHomePageState라는 State 클래스를 생성합니다.

```dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
```

해당 클래스에는 build를 오버라이딩하여 화면에 그려줄 위젯들을 정의해줍니다.

```dart
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
```

먼저 세로를 기준으로 가운데 정렬합니다.

```dart
          children: <Widget>[
            MaterialButton(
                onPressed: () {
                  showAboutDialog(
                      context: context,
                      applicationName: 'application',
                      applicationLegalese: '© 2020 author',
                      applicationVersion: "1.0.0",
                      applicationIcon: Icon(Icons.public),
                      children: [
                        OutlineButton(
                            onPressed: () {}, child: Text("test button")),
                      ]);
                },
                child: Text("test"))
          ]),
    ));
  }
}
```

세로로 정렬된 Material Button을 누르면 애플리케이션 이름, 버전, 아이콘을 지정한대로 showAboutDialog 위젯이 띄어지게 됩니다.

추가로 하단의 VIEW LICENSES 버튼을 누르면 앱에서 사용하고 있는 라이선스들이 출력되게 됩니다.