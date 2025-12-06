---
layout: post
title: flutter device_info 패키지 사용하여 기기 정보 알아보기
---

오늘은 flutter로 앱이 실행되고 있는 기기의 정보를 알아보는 device_info 패키지에 대하여 알아보려 합니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱으로 테스트할 장치를 준비해야 합니다.

준비했으면, 미리 기기 또는 에뮬레이터를 ide에 연결해줍니다.

## pubspec.yaml 작성하기

```yaml
dependencies:
  flutter:
    sdk: flutter
  device_info:
```

flutter 앱에서 기기 정보를 알아내려면 device_info 패키지를 pubspec.yaml에 작성해줍니다.

## main.dart 앱 코드 작성하기

```dart
import 'package:flutter/material.dart';
import 'package:device_info/device_info.dart';
import 'dart:io';
```

앱에 머티리얼 위젯을 추가할 수 있는 material 패키지와 기기 정보를 알 수 있는 device_info 패키지를 가져옵니다.

```dart
void main() {
  runApp(MyApp());
}
```

앱을 MyApp으로 구동합니다.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter device_info',
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
  List _data = <dynamic>[];
  static final DeviceInfoPlugin plugin = DeviceInfoPlugin();
```

기기의 정보를 담을 리스트와 DeviceInfoPlugin 객체를 생성해줍니다.

```dart
  @override
  void initState() {
    super.initState();
    initPlatform();
  }
```

오버라이딩한 initState에 의해 앱이 실행될 때 initPlatform 함수가 수행됩니다.

```dart
  Future<void> initPlatform() async {
    if (Platform.isAndroid) {
      setState(() async {
        _data = getAndroidDevice(await plugin.androidInfo);
      });
    }
    if (Platform.isIOS) {
      setState(() async {
        _data = getIosDevice(await plugin.iosInfo);
      });
    }
  }
```

initPlatform은 앱이 실행되는 기기의 플랫폼에 따라 가져오는 정보들이 다르도록 구별해줍니다.

비동기로 받아와서 setState에 의해 화면을 새로 그려줍니다.

```dart
  getAndroidDevice(AndroidDeviceInfo device) {
    return [
      device.version.securityPatch,
      device.version.sdkInt,
      device.version.release,
      device.version.codename,
      device.board,
      device.bootloader,
      device.brand,
      device.device,
      device.display,
      device.fingerprint,
      device.hardware,
      device.host,
      device.id,
      device.manufacturer,
      device.model,
      device.product,
      device.androidId,
    ];
  }
```

안드로이드와 같은 경우에는 위와 같은 정보들을 가져올 수 있으며, 위 코드들은 모든 정보를 표시한 것은 아닙니다.

```dart
  getIosDevice(IosDeviceInfo device) {
    return [
      device.name,
      device.systemName,
      device.systemVersion,
      device.model,
      device.localizedModel,
      device.identifierForVendor,
    ];
  }
```

ios도 정보를 가져올 수 있으며, 이 역시 모든 정보를 코드에 표기한 것은 아닙니다.

```dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Text(
          _data.toString(),
        ),
      ),
    );
  }
}
```

initPlatform에서 받아온 정보들로 화면 가운데에 그려줍니다.
