---
layout: post
title: flutter의 mockito로 http 테스트 해보기
---

오늘은 flutter에서 http 테스트로 mockito를 사용하여 유닛 테스트해보려 합니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱으로 테스트할 장치를 준비해야 합니다.

준비했으면, 미리 기기 또는 에뮬레이터를 ide에 연결해줍니다.

## pubspec.yaml 작성하기

```yaml
dependencies:
  flutter:
    sdk: flutter
  http:

dev_dependencies:
  flutter_test:
    sdk: flutter
  mockito:
```

유닛 테스트를 위한 패키지와 mockito 패키지, 그리고 http 패키지를 pubspec 파일의 의존성에 추가해줍니다.


## main.dart 앱 코드 작성하기

메인 코드를 작성해야 합니다.

메인 코드는 이전에 작성한 "flutter로 인터넷에서 json 데이터 가져오기"에서 참고합니다.

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
Future<TextClass> fetch(http.Client client) async {
  final res = await http.get('https://postman-echo.com/response-headers?title=blog_post');
  return TextClass(title: json.decode(res.body)['title']);
}
```

이전 포스팅에서 작성한 내용에서 유닛 테스트를 진행할 기능을 함수로 따로 빼둡니다.

http.get 메소드는 비동기식으로 json 데이터를 가져오게 되며, 값이 들어오면 json을 분해해서 Text 생성자로 반환하게 됩니다.

함수에 mock 객체가 들어가 작동해야 하므로 파라미터를 생성해줍니다.

```dart
final mainClient = http.Client();
```

앱에서는 구현되있는 Client 객체를 넣어줍니다.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Center(
      child: FutureBuilder<TextClass>(
        future: fetch(mainClient),
```

FutureBuilder로 데이터를 가져와서 앱에 출력할 수 있습니다.

future에는 방금 만든 fetch 함수를 넣어줍니다.

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

## 테스트 코드 작성하기

```dart
import 'package:http/http.dart' as http;
import 'package:mockito/mockito.dart';
import 'package:flutter_test/flutter_test.dart';
import 'package:http_mock_test/main.dart';
```

mockito로 네트워크에서 데이터를 가져오는 것을 테스트하기 위해 http와 mockito 패키지를 가져옵니다.

```dart
class MockClient extends Mock implements http.Client {}
```

http.Client 클래스로 mock 클래스를 구성합니다.

```dart
main() {
  group('mockito http test', () {
    test('return title string', () async {
      final client = MockClient();
```

mock 객체를 생성합니다.

```dart
      when(client
              .get('https://postman-echo.com/response-headers?title=blog_test'))
          .thenAnswer(
              (_) async => http.Response('{"title": "blog_test"}', 200));
```

thenAnswer를 이용하여 특정 주소를 get했을 때에 원하는 응답을 mock에 직접 설정해줄 수 있습니다.

```dart
      expect(await fetch(client), isInstanceOf<TextClass>());
    });
  });
}
```

메인 코드의 fetch 함수에 mock 객체를 넣어서 반환되었을 때에  반환 값이 TextClass 타입의 변수가 맞으면 테스트에 통과하게 됩니다.

## mock 객체로 테스트 진행하기

```bash
flutter test
```

해당 프로젝트의 모든 테스트를 실행하고 싶다면 위와 같이 입력합니다.

```bash
flutter test test/main_test.dart
```

이와 같은 경우에는 mockito가 사용된 test 폴더의 main_test 파일만 테스트하게 합니다.

모든 테스트가 마무리되면 자동으로 종료됩니다.