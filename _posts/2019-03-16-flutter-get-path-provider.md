---
layout: post
title: flutter path_provider 사용하여 폴더 경로 가져오기
---

오늘은 flutter로 앱이 설치되어 있는 경로와 임시 폴더의 경로를 찾을 수 있는 path_provider 패키지에 대하여 알아보려 합니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱으로 테스트할 장치를 준비해야 합니다.

준비했으면, 미리 기기 또는 에뮬레이터를 ide에 연결해줍니다.

## pubspec.yaml 작성하기

```yaml
dependencies:
  flutter:
    sdk: flutter
  path_provider:
```

flutter 앱에 앱의 경로와 앱이 사용하고 있는 임시 폴더의 경로를 알아내기 위해서 path_provider 패키지를 pubspec.yaml에 작성해줍니다.

## main.dart 앱 코드 작성하기

```dart
import 'package:flutter/material.dart';
import 'package:path_provider/path_provider.dart';
import 'dart:io';
```

앱에 머티리얼 위젯을 추가할 수 있는 material 패키지와 패키지의 정보를 추가할 수 있게 해주는 path_provider 패키지를 가져옵니다.

그리고 디렉토리를 가져오기 위해 다트 패키지인 io도 가져와줍니다.

```dart
void main() => runApp(MyApp());
```

앱을 MyApp으로 구동합니다.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter path_provider',
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
  Future<Directory> _tempDir;
  Future<Directory> _appDir;
  Future<Directory> _externalDir;
```

임시 폴더, 앱 폴더, 외장 메모리 폴더를 Directory 형태인 Future로 선언해줍니다.

```dart
  @override
  void initState() {
    super.initState();
    _requestTempDir();
    _requestAppDir();
    _requestExternalDir();
  }
```

initState를 오버라이딩하여 임시, 앱, 외장 메모리 디렉토리를 요청합니다.

```dart
  void _requestTempDir() {
    setState(() {
      _tempDir = getTemporaryDirectory();
    });
  }
```

getTemporaryDirectory 를 수행하여 \_tempDir에 임시 파일 디렉토리를 반환합니다.

```dart
  void _requestAppDir() {
    setState(() {
      _appDir = getApplicationDocumentsDirectory();
    });
  }
```

getApplicationDocumentsDirectory 를 수행하여 \_appDir에 앱 문서 디렉토리를 반환합니다.

```dart
  void _requestExternalDir() {
    setState(() {
      _externalDir = getExternalStorageDirectory();
    });
  }
```

getExternalStorageDirectory 를 수행하여 \_externalDir에 외장 메모리 디렉토리를 반환합니다.

```dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: <Widget>[
          FutureBuilder<Directory>(
            future: _tempDir,
            builder: (BuildContext context, AsyncSnapshot<Directory> snapshot) {
              if (snapshot.connectionState == ConnectionState.done) {
                if (snapshot.hasData) {
                  return Text('path: ${snapshot.data.path}');
                } else {
                  return const Text('path unavailable');
                }
              }
            },
          ),
```

builder에 의하여 \_tempDir의 future를 스냅샷으로 전달합니다.

만약 snapshot.hasData으로 데이터가 존재하면 snapshot.data.path을 출력해서 임시 파일 디렉토리 경로를 문자열로 출력하게 됩니다.

```dart
          FutureBuilder<Directory>(
            future: _appDir,
            builder: (BuildContext context, AsyncSnapshot<Directory> snapshot) {
              if (snapshot.connectionState == ConnectionState.done) {
                if (snapshot.hasData) {
                  return Text('path: ${snapshot.data.path}');
                } else {
                  return const Text('path unavailable');
                }
              }
            },
          ),
```

builder에 의하여 \_appDir의 future를 스냅샷으로 전달합니다.

만약 snapshot.hasData으로 데이터가 존재하면 snapshot.data.path을 출력해서 앱 디렉토리 경로를 문자열로 출력하게 됩니다.

```dart
          FutureBuilder<Directory>(
            future: _externalDir,
            builder: (BuildContext context, AsyncSnapshot<Directory> snapshot) {
              if (snapshot.connectionState == ConnectionState.done) {
                if (snapshot.hasData) {
                  return Text('path: ${snapshot.data.path}');
                } else {
                  return const Text('path unavailable');
                }
              }
            },
          ),
        ],
      ),
    );
  }
}
```

builder에 의하여 \_externalDir의 future를 스냅샷으로 전달합니다.

만약 snapshot.hasData으로 데이터가 존재하면 snapshot.data.path을 출력해서 외장 메모리 경로를 문자열로 출력하게 됩니다.
