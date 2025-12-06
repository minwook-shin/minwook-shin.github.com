---
layout: post
title: flutter로 battery 확인하기
---

오늘은 flutter로 앱이 실행되고 있는 기기의 배터리 상태를 알 수 있는 방법을 실습해보려 합니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱으로 테스트할 장치를 준비해야 합니다.

준비했으면, 미리 기기 또는 에뮬레이터를 ide에 연결해줍니다.

## pubspec.yaml 작성하기

```yaml
dependencies:
  flutter:
    sdk: flutter
  battery:
```

flutter에서 battery 상태 확인을 구현하려면 battery 패키지를 pubspec.yaml에 작성해줍니다.

## main.dart 앱 코드 작성하기

```dart
import 'package:flutter/material.dart';
import 'package:battery/battery.dart';
```

앱에 머티리얼 위젯을 추가할 수 있는 material 패키지와 배터리 상태를 알 수 있는 battery 패키지를 가져옵니다.

```dart
void main() => runApp(MyApp());
```

main 함수를 작성하고 앱이 실행되면 MyApp이 수행되도록 합니다.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
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
  var battery = Battery();
  BatteryState _batteryState;
```

Battery 객체를 만들어주고, 배터리 상태를 담아줄 BatteryState 변수도 만들어줍니다.

```dart
  @override
  void initState() {
    super.initState();
    battery.onBatteryStateChanged.listen((BatteryState state) {
      setState(() {
        _batteryState = state;
      });
    });
  }
```

오버라이딩된 initState 메소드로 인해 onBatteryStateChanged 이벤트 리스너가 구동됩니다.

이 이벤트 리스너가 앱의 배터리 상태 변화를 확인하여 BatteryState 변수에 담아줍니다.

변수에 담길 때에는 setState에 의해 화면이 새로 그려집니다.

```dart
  getBatteryLevel() async {
    final int batteryLevel = await battery.batteryLevel;
    return batteryLevel;
  }
```

getBatteryLevel라는 비동기 함수를 만들어줍니다.

batteryLevel 상수에 batteryLevel이 담기게 되면서 함수가 종료되면 반환됩니다.

```dart
  @override
  Widget build(BuildContext context) {
    final key = new GlobalKey<ScaffoldState>();
```

GlobalKey를 만들어줍니다.

```dart
    return Scaffold(
      key: key,
```

Scaffold의 키로 GlobalKey를 등록해서 snackbar로 사용하도록 해줍니다.

```dart
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text(_batteryState.toString()),
```

앱의 정가운데에 배터리의 상태가 문자열로 출력됩니다.

```dart
            OutlineButton(
              child: Icon(Icons.battery_charging_full),
              onPressed: () async {
                final int batteryLevel = await battery.batteryLevel;
                key.currentState.showSnackBar(
                    SnackBar(content: Text(batteryLevel.toString())));
              },
            )
          ],
        ),
      ),
    );
  }
}
```

배터리 모양의 아이콘을 가진 버튼을 누르게되면 onPressed 콜백에 등록된 비동기 익명 함수에 의해서 배터리 레벨이 snackbar로 출력되게 됩니다.
