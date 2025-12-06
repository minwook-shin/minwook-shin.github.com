---
layout: post
title: flutter connectivity 패키지 사용하여 네트워크 상태 알아보기
---

오늘은 flutter로 앱이 실행되고 있는 기기의 네트워크 상태를 알아보는 connectivity 패키지에 대하여 알아보려 합니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱으로 테스트할 장치를 준비해야 합니다.

준비했으면, 미리 기기 또는 에뮬레이터를 ide에 연결해줍니다.

## pubspec.yaml 작성하기

```yaml
dependencies:
  flutter:
    sdk: flutter
  connectivity:
```

flutter 앱에서 네트워크 상태를 판단하려면 connectivity 패키지를 pubspec.yaml에 작성해줍니다.

## main.dart 앱 코드 작성하기

```dart
import 'package:flutter/material.dart';
import 'package:connectivity/connectivity.dart';
```

앱에 머티리얼 위젯을 추가할 수 있는 material 패키지와 네트워크 상태를 알 수 있는 connectivity 패키지를 가져옵니다.

```dart
void main() => runApp(MyApp());
```

앱을 MyApp으로 구동합니다.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter connectivity',
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
  String networkStatus = 'null';
```

networkStatus를 초기화 합니다.

```dart
  @override
  void initState() {
    super.initState();
    initConnect();
  }
```

initState를 오버라이딩하여 앱이 구동될 때에 initConnect 비동기 함수를 수행하게 합니다.

```dart
  initConnect() async {
    ConnectivityResult connectResult;
    connectResult = await Connectivity().checkConnectivity();
    updateStatus(connectResult);
  }
```

네트워크의 상태를 담을 ConnectivityResult 타입의 변수를 만들어서 checkConnectivity()로 기기의 무선 상태를 담아줍니다.

```dart
  updateStatus(ConnectivityResult result) async {
    if (result == ConnectivityResult.wifi) {
      String wifiName, wifiIP;
      wifiName = (await Connectivity().getWifiName()).toString();
      wifiIP = (await Connectivity().getWifiIP()).toString();
```

네트워크의 상태가 담긴 result로 와이파이일 경우의 조건문으로 와이파이 이름과 ip를 조회합니다.

```dart
      setState(() {
        networkStatus = result.toString() + "/" + wifiName + "/" + wifiIP;
      });
```

wifi의 상태일 때 setState에 의해 networkStatus이 바뀌면서 화면이 다시 그려집니다.

```dart
    } else if (result == ConnectivityResult.mobile) {
      setState(() {
        networkStatus = result.toString();
      });
```

모바일 데이터의 상태일 때 setState에 의해 networkStatus이 바뀌면서 화면이 다시 그려집니다.

```dart
    } else if (result == ConnectivityResult.none) {
      setState(() => networkStatus = result.toString());
    }
  }
```

네트워크가 끊겨 있을 때 setState에 의해 networkStatus이 바뀌면서 화면이 다시 그려집니다.

```dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(child: Text(networkStatus)),
    );
  }
}
```

Center에 의해 화면의 가운데로 networkStatus 문자열이 출력됩니다.
