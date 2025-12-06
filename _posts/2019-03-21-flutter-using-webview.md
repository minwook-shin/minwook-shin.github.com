---
layout: post
title: flutter 웹뷰 추가하기
---

오늘은 flutter로 웹뷰 위젯을 추가할 수 있는 webview_flutter 패키지에 대하여 알아보려 합니다.

안드로이드와 ios 모두 사용 가능하지만, 안드로이드에는 WebView, ios에는 WKWebView가 들어갑니다.

## WKWebView

```
Starting in iOS 8.0 and OS X 10.10, use WKWebView to add web content to your app. Do not use UIWebView or WebView.
```

애플 공식 개발자 문서에 따르면 iOS 8.0부터는 UIWebView를 사용하지 않고, WKWebView를 사용하라고 안내하고 있습니다.

UIWebView보다 속도가 더 빠르다고 합니다.

## iOS

Info.plist 파일에 io.flutter.embedded_views_preview 키의 값을 true로 만들어야 합니다.

```xml
<key>io.flutter.embedded_views_preview</key>
	<true/>
```

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱으로 테스트할 장치를 준비해야 합니다.

준비했으면, 미리 기기 또는 에뮬레이터를 ide에 연결해줍니다.

## pubspec.yaml 작성하기

```yaml
dependencies:
  flutter:
    sdk: flutter
  webview_flutter:
```

flutter 앱에 웹뷰를 추가하기 위하여 webview_flutter 패키지를 pubspec.yaml에 작성해줍니다.

## main.dart 앱 코드 작성하기

```dart
import 'package:flutter/material.dart';
import 'package:webview_flutter/webview_flutter.dart';
```

앱에 머티리얼 위젯을 추가할 수 있는 material 패키지와 웹뷰를 추가해줄 수 있는 webview_flutter 패키지를 가져옵니다.

```dart
void main() => runApp(MyApp());
```

MyApp 객체를 앱으로 구동합니다.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter webview',
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
```

\_MyHomePageState라는 State 클래스를 생성합니다.

```dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
```

해당 클래스에는 build를 오버라이딩하여 화면에 그려줄 위젯들을 정의해줍니다.

```dart
      body: WebView(
        initialUrl: 'https://flutter.dev',
```

해당 프로젝트는 webview_flutter 패키지를 추가했기 때문에 WebView 위젯을 사용할 수 있습니다.

initialUrl 속성으로 처음 열 url을 지정합니다.

```dart
        javascriptMode: JavascriptMode.unrestricted,
```

javascriptMode 속성으로 자바스크립트를 웹뷰에서 허용할지 불허할 지에 대하여 선택할 수 있습니다.

모드는 unrestricted와 disabled가 존재합니다.

```dart
        onPageFinished: (String url) {
          print('finished:' + url);
        },
      ),
    );
  }
}
```

onPageFinished 콜백을 지정하여 해당 웹뷰가 불러와졌을 때에 수행할 작업을 정의해줍니다.
