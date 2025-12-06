---
layout: post
title: flutter 패키지 정보 가져오기
---

오늘은 flutter로 패키지의 정보를 가져올 수 있는 package_info 패키지에 대하여 알아보려 합니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱으로 테스트할 장치를 준비해야 합니다.

준비했으면, 미리 기기 또는 에뮬레이터를 ide에 연결해줍니다.

## pubspec.yaml 작성하기

```yaml
dependencies:
  flutter:
    sdk: flutter
  package_info:
```

flutter 패키지에 대한 정보를 가져오기 위해서 package_info 패키지를 pubspec.yaml에 작성해줍니다.

## main.dart 앱 코드 작성하기

```dart
import 'package:flutter/material.dart';
import 'package:package_info/package_info.dart';
```

앱에 머티리얼 위젯을 추가할 수 있는 material 패키지와 패키지의 정보를 추가할 수 있게 해주는 package_info 패키지를 가져옵니다.

```dart
void main() => runApp(MyApp());
```

앱을 MyApp으로 구동합니다.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter package_info',
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
  var _packageInfo = PackageInfo();
```

PackageInfo 객체를 생성해줍니다.

```dart
  @override
  initState() {
    super.initState();
    getPackageInfo();
  }
```

initState를 오버라이딩하여 앱이 구동했을 때에 getPackageInfo 함수를 수행하게 합니다.

```dart
  getPackageInfo() async {
    var packageInfo = await PackageInfo.fromPlatform();
```

getPackageInfo 함수는 PackageInfo에서 fromPlatform로 인해 패키지의 정보들을 받아오게 됩니다.

```dart
    setState(() {
      _packageInfo = packageInfo;
    });
  }
```

packageInfo가 변수에 담기면서 setState에 의해서 화면이 다시 그려지게 됩니다.

```dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Column(
        children: <Widget>[
          Text(_packageInfo.appName ?? 'Not set'),
          Text(_packageInfo.packageName ?? 'Not set'),
          Text(_packageInfo.version ?? 'Not set'),
          Text(_packageInfo.buildNumber ?? 'Not set'),
        ],
      ),
    );
  }
}
```

화면에 앱 이름, 패키지 이름, 버전, 빌드번호가 출력되며 존재하지 않을 경우에는 'Not set'으로 표기됩니다.

## async mode

```dart
PackageInfo.fromPlatform().then((PackageInfo packageInfo) {
  String appName = packageInfo.appName;
  String packageName = packageInfo.packageName;
  String version = packageInfo.version;
  String buildNumber = packageInfo.buildNumber;
});
```

async 모드에서는 await PackageInfo.fromPlatform() 대신 위와 같이 작성할 수도 있습니다.
