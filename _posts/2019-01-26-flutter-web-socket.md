---
layout: post
title: flutter로 Web Socket 사용하기
---

오늘은 flutter에서 Web Socket을 이용하여 앱을 만들어보려 합니다.

이 포스팅을 따라 오신다면 여러분도 Web Socket를 사용하여 서버에 연결하는 앱을 안드로이드, ios 앱으로 동시에 만들어 볼 수 있습니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱을 구동할 장치를 준비해야 합니다.

준비했으면, 미리 기기를 ide에 연결해줍니다.

## pubspec.yaml 작성하기

web_socket_channel 외부 패키지를 pubspec 파일의 의존성에 추가해줍니다.

```yaml
dependencies:
  flutter:
    sdk: flutter
  web_socket_channel:
```

이 앱에서는 데이터를 서버로 전송하고, 다시 돌려받는 역할로 쓰입니다.

## main.dart 작성하기

이제 메인 코드를 작성해야 합니다.

```dart
import 'package:web_socket_channel/io.dart';
import 'package:flutter/material.dart';
import 'package:web_socket_channel/web_socket_channel.dart';
```

소켓 통신을 위해 io와 web_socket_channel 패키지를 가져오고, material 패키지도 가져와줍니다.

```dart
void main() {
  runApp(MaterialApp(
    title: 'WebSocket', 
    home: Scaffold(body: MyApp())));
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

createState 메소드를 오버라이딩하여 MyAppState 객체를 생성해줍니다.

```dart
class _MyAppState extends State<MyApp> {
  TextEditingController _controller = TextEditingController();
```

TextEditing Controller를 만들어서 텍스트 폼의 텍스트를 가져올 때에 사용합니다.

```dart
  final WebSocketChannel ch =
      IOWebSocketChannel.connect('ws://echo.websocket.org');
```

웹 소켓 채널을 만들기 위해서 IOWebSocketChannel.connect 메소드를 사용해줍니다.

예제는 echo.websocket.org라는 서버로 입력한 텍스트 값을 다시 받습니다.

```dart
  @override
  void dispose() {
    ch.sink.close();
    super.dispose();
  }
```

앱이 dispose될 때에 웹 소켓의 연결을 끊어줍니다.

```dart
  @override
  Widget build(BuildContext context) {
    return Column(
      children: <Widget>[
        TextFormField(
          controller: _controller,
        ),
```

TextFormField을 만들어서 이전에 만든 컨트롤러를 등록해줍니다.

이를 이용하여 버튼을 눌렀을 때에 해당 텍스트 폼에서 문자열을 가져옵니다.

```dart
        StreamBuilder(
          stream: ch.stream,
```

StreamBuilder를 이용하여 소켓 통신으로 새로운 텍스트가 기기에 들어오게 되면, 해당 문자열을 앱에 출력하도록 합니다.

future와는 다르게 stream을 사용하면 데이터를 비동기로 받을 수 있고, 많은 이벤트를 처리할 수 있습니다.

```dart
          builder: (context, snapshot) {
            if (snapshot.hasData) {
              return Text(snapshot.data.toString());
            }
            return Text("");
          },
        ),
```

텍스트가 앱으로 들어오게 된다면, Text 생성자로 앱에 문자열을 출력합니다.

처음 앱을 구동했을 때에는 snapshot의 데이터가 존재하지 않으므로 빈 값의 Text 생성자가 반환됩니다.

```dart
        OutlineButton(
          onPressed: () {
            ch.sink.add(_controller.text);
          },
          child: Icon(Icons.send),
        )
      ],
    );
  }
}
```

버튼을 누르면, sink.add 메소드로 인하여 텍스트 폼의 문자열을 서버에 전송하게 됩니다.