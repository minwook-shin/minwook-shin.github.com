---
layout: post
title: flutter에서 쓰고 읽는 파일로 문자열 저장 구현하기
---

오늘은 flutter에서 글자를 저장하는 방식중 하나인 기기 내부에 파일을 저장하고 읽는 앱을 만들어보려 합니다.

이 포스팅을 따라 오신다면 여러분도 기기의 내부에 자신이 저장하고 싶은 내용을 파일로 저장하고 읽는 앱을 안드로이드, ios 앱으로 동시에 만들어 볼 수 있습니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱을 구동할 장치를 준비해야 합니다.

준비했으면, 미리 기기를 ide에 연결해줍니다.

## pubspec.yaml 작성하기

파일을 쓸 경로를 가져오기 위해 path_provider 외부 패키지를 pubspec 파일의 의존성에 추가해줍니다.

```yaml
dependencies:
  flutter:
    sdk: flutter
  path_provider:
```

getApplicationDocumentsDirectory 메소드를 사용할 수 있습니다.

## main.dart 작성하기

이제 메인 코드를 작성해보아야 합니다.

```dart
import 'dart:io';
import 'package:flutter/material.dart';
import 'package:path_provider/path_provider.dart';
```

dart:io와 package:path_provider 패키지를 이용하여 파일을 읽고 쓸 준비합니다.

material 패키지로 머티리얼 디자인을 가져옵니다.

```dart
void main() {
  runApp(
    MaterialApp(
      title: 'Reading Writing Files', 
      home: Scaffold(body: MyApp())),
  );
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
  int _counter;
```

_counter 변수로 숫자가 카운팅되는 것을 저장해줍니다.

변수 앞에 밑줄로 인하여 변수가 private하다고 알려줍니다.

```dart
  Future read() async {
    try {
      final dir = await getApplicationDocumentsDirectory();
      return int.parse(
          await File(dir.path + '/counter.txt').readAsString());
    } catch (e) {
      return 0;
    }
  }
```

비동기 함수인 read를 만들어주고, getApplicationDocumentsDirectory로 가져온 경로에 counter.txt 파일을 읽어들이게 합니다.

읽어들인 문자를 int로 파싱하여 반환해줍니다.

try catch문으로 예외처리도 해줍니다.

```dart
  Future write(int counter) async {
    final dir = await getApplicationDocumentsDirectory();
    return File(dir.path + '/counter.txt')
        .writeAsString(counter.toString());
  }
```

비동기 함수인 write를 만들어주고, getApplicationDocumentsDirectory로 가져온 경로에 counter.txt 파일을 쓰게 합니다.

writeAsString 메소드로 인하여 카운팅된 수가 문자열로 변환된 뒤에 파일에 저장되게 됩니다.

```dart
  @override
  void initState() {
    super.initState();
    read().then((value) {
      setState(() {
        _counter = value;
      });
    });
  }
```

initState 메소드를 오버라이딩하여 앱이 처음 구동될 때에 read 함수를 수행하고, setState 메소드로 인해 값이 내부 변수인 _counter로 들어가 화면이 새로 고쳐집니다.

```dart
  @override
  Widget build(BuildContext context) {
    return ListView(children: [
      Text(_counter.toString(), textAlign: TextAlign.center),
```

ListView로 반환하여 내부 변수인 _counter를 나타낼 Text 생성자와 버튼을 만들어줍니다.

```dart
      OutlineButton(
          onPressed: () {
            setState(() {
              _counter++;
            });
            return write(_counter);
          },
          child: Icon(Icons.add)),
```

onPressed 콜백으로 버튼이 눌리면, setState에 의해 _counter 값이 증가하고 화면이 새로 그려지게 됩니다.

화면이 새로 그려지게 되면, 파일에 값을 써주고 익명 함수를 반환합니다.

```dart
      OutlineButton(
          onPressed: () {
            setState(() {
              _counter--;
            });
            return write(_counter);
          },
          child: Icon(Icons.remove))
    ]);
  }
}
```

onPressed 콜백으로 버튼이 눌리면, setState에 의해 _counter 값이 감소하고 화면이 새로 그려지게 됩니다.

화면이 새로 그려지게 되면, 파일에 값을 써주고 익명 함수를 반환합니다.