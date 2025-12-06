---
layout: post
title: flutter url 열 수 있는 패키지 사용하기
---

오늘은 flutter로 url을 열 수 있는 url_launcher 패키지에 대하여 알아보려 합니다.

안드로이드와 ios 모두 사용 가능하므로 in-app 조작에서만 안드로이드에서 웹뷰, ios에는 SFSafariViewController라는 차이점이 있습니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱으로 테스트할 장치를 준비해야 합니다.

준비했으면, 미리 기기 또는 에뮬레이터를 ide에 연결해줍니다.

## pubspec.yaml 작성하기

```yaml
dependencies:
  flutter:
    sdk: flutter
  url_launcher:
```

flutter 앱에 웹 url을 열 수 있는 기능을 위하여 url_launcher 패키지를 pubspec.yaml에 작성해줍니다.

## Scheme

기본적으로 지원되는 Scheme는 아래와 같습니다.

- http , https : 브라우저 앱

- mailto : 이메일 앱

- tel : 기본 전화 앱

- sms : 메시징 앱

## main.dart 앱 코드 작성하기

```dart
import 'package:flutter/material.dart';
import 'package:url_launcher/url_launcher.dart';
```

앱에 머티리얼 위젯을 추가할 수 있는 material 패키지와 url을 열어 줄 share 패키지를 가져옵니다.

```dart
void main() => runApp(MyApp());
```

MyApp 객체를 앱으로 구동합니다.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter url_launcher',
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
  launchBrowser(String url) async {
    if (await canLaunch(url)) {
      await launch(url, forceSafariVC: false, forceWebView: false);
    }
  }
```

launchBrowser라는 비동기 함수를 만들고 우선 canLaunch로 해당 url 스키마가 기기에서 지원되는지 확인해줍니다.

만약 지원한다면 launch로 해당 url을 외부 브라우저로 열어줍니다.

안드로이드에서는 forceWebView, ios에서는 forceSafariVC만 적용되므로 두 플랫폼에서 모두 외부 브라우저로 열려면 forceWebView, forceSafariVC를 false로 해야 합니다.

```dart
  launchWebView(String url) async {
    if (await canLaunch(url)) {
      await launch(url, forceSafariVC: true, forceWebView: true);
    }
  }
```

launchWebView라는 비동기 함수를 만들고 우선 canLaunch로 해당 url 스키마가 기기에서 지원되는지 확인해줍니다.

만약 지원한다면 launch로 해당 url을 앱 내부에서 In-app으로 열어줍니다.

안드로이드에서는 forceWebView, ios에서는 forceSafariVC만 적용되므로 두 플랫폼에서 모두 In-app으로 열려면 forceWebView, forceSafariVC를 true로 해야 합니다.

```dart
  launchUniversalForIos(String url) async {
    if (await canLaunch(url)) {
      final bool succeeded =
          await launch(url, forceSafariVC: false, universalLinksOnly: true);
      if (!succeeded) {
        await launch(url, forceSafariVC: true);
      }
    }
  }
```

ios에서 지원하는 유니버셜 링크를 적용할 수 있습니다.

하지만 유니버셜 링크가 지원하지 않을 수 있기 때문에 universalLinksOnly 옵션을 준 상태로 가능한지 확인하고 만약 불가능한다면 In-app으로 열 수 있게 바꿔줍니다.

```dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            OutlineButton(
              onPressed: () {
                setState(() {
                  launchBrowser("https://www.google.com");
                });
              },
              child: Column(
                children: <Widget>[Icon(Icons.link), Text("launchBrowser")],
              ),
            ),
```

버튼의 onPressed 콜백으로 버튼이 눌리면 외부 브라우저로 url을 열어줍니다.

이 때 setState에 의하여 화면이 새로 고쳐집니다.

```dart
            OutlineButton(
              onPressed: () {
                setState(() {
                  launchWebView("http://www.google.com");
                });
              },
              child: Column(
                children: <Widget>[Icon(Icons.link), Text("launchWebView")],
              ),
            ),
```

버튼의 onPressed 콜백으로 버튼이 눌리면 앱 내부의 웹뷰로 url을 열어줍니다.

이 때 setState에 의하여 화면이 새로 고쳐집니다.

```dart
            OutlineButton(
              onPressed: () {
                launchUniversalForIos("http://youtube.com");
              },
              child: Column(
                children: <Widget>[
                  Icon(Icons.link),
                  Text("launchUniversalForIos")
                ],
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

버튼의 onPressed 콜백으로 버튼이 눌리면 ios에서 유니버셜 링크로 url을 여는 시도를 합니다.

이 때 setState에 의하여 화면이 새로 고쳐집니다.
