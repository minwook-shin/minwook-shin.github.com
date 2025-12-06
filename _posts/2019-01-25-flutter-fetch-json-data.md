---
layout: post
title: flutter로 인터넷에서 json 데이터 가져오기
---

오늘은 flutter에서 json 데이터를 http로 가져와서 출력하는 앱을 만들어보려 합니다.

이 포스팅을 따라 오신다면 여러분도 인터넷에 있는 json 데이터들을 가져와서 출력하는 앱을 안드로이드, ios 앱으로 동시에 만들어 볼 수 있습니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱을 구동할 장치를 준비해야 합니다.

준비했으면, 미리 기기를 ide에 연결해줍니다.

## pubspec.yaml 작성하기

http 외부 패키지를 pubspec 파일의 의존성에 추가해줍니다.

```yaml
dependencies:
  flutter:
    sdk: flutter
  http:
```

인터넷에서 데이터를 받아올 때에 유용한 패키지입니다.

## main.dart 작성하기

이제 메인 코드를 작성해야 합니다.

```dart
import 'dart:convert';
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
```

json 데이터를 변환해줄 convert 패키지와 material 패키지, 그리고 pubspec 파일에 추가해준 http 외부 패키지도 가져옵니다.

```dart
class TextClass {
  String title;
  TextClass({this.title});
}
```

title 문자열 필드를 가진 클래스를 만들어줍니다.

생성자까지 만들어서 나중에 받을 json 데이터의 형식을 decode 해줄 수 있게 해줍니다.

```dart
void main() {
  runApp(MaterialApp(
    title: 'Fetch Data', 
    home: Scaffold(body: MyApp())));
}
```

처음 앱이 실행될 때 앱의 타이틀과 메인스크린 위젯을 출력합니다.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Center(
      child: FutureBuilder<TextClass>(
        future: (() async {
          final res =
              await http.get('http://echo.jsontest.com/title/blog-posting');
          return TextClass(title: json.decode(res.body)['title']);
        })(),
```

FutureBuilder로 데이터를 가져와서 앱에 출력할 수 있습니다.

http.get 메소드는 비동기식으로 json 데이터를 가져오게 됩니다.

값이 들어오면 json을 분해해서 Text 생성자로 반환하게 됩니다.

```dart
        builder: (context, snapshot) {
          if (snapshot.hasData) {
            return Text(snapshot.data.title);
          } else if (snapshot.hasError) {
            return Text(snapshot.error.toString());
          }
```

Future에서 값을 받아오는 응답코드에 따라 값을 다르게 반환할 수 있습니다.

데이터가 있다면 snapshot.data의 title 문자열이 반환되고, 없다면 오류 메시지가 반환됩니다.

```dart
          return LinearProgressIndicator();
        },
      ),
    );
  }
}
```

데이터가 받아지기 전까지는 Progress 바가 동작합니다.