---
layout: post
title: flutter 기기 센서 사용하기
---

오늘은 flutter로 기기의 센서를 사용하게 해주는 sensors 패키지에 대하여 알아보려 합니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱으로 테스트할 장치를 준비해야 합니다.

준비했으면, 미리 기기 또는 에뮬레이터를 ide에 연결해줍니다.

## pubspec.yaml 작성하기

```yaml
dependencies:
  flutter:
    sdk: flutter
  sensors:
```

flutter 앱에 기기의 자이로스코프, 가속도계 센서를 사용하기 위하여 sensors 패키지를 pubspec.yaml에 작성해줍니다.

## 자이로 스코프

모든 각도의 변화를 측정할 수 있으므로 회전 운동의 각도를 알 수 있습니다.

보통 3개의 축으로 인식하게 됩니다.

## 가속도계

가속도의 물리량을 측정할 수 있으므로, 지표면을 중심으로 기울기와 가속도를 측정하게 됩니다.

## main.dart 앱 코드 작성하기

```dart
import 'package:flutter/material.dart';
import 'package:sensors/sensors.dart';
```

앱에 머티리얼 위젯을 추가할 수 있는 material 패키지와 기기의 센서를 측정할 sensors 패키지를 가져옵니다.

```dart
void main() => runApp(MyApp());
```

앱을 MyApp으로 구동합니다.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter sensors',
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
  List accelerometer;
  List gyroscope;
```

가속도계와 자이로스코프 센서 값을 담을 리스트 변수를 선언합니다.

x축, y축, z축이 순서대로 담기게 될 것 입니다.

```dart
  @override
  void initState() {
    super.initState();
```

initState을 오버라이딩하여 앱이 실행될 때 시작될 작업을 정의해줍니다.

```dartv
    accelerometerEvents.listen((AccelerometerEvent e) {
      setState(() {
        accelerometer = <double>[e.x, e.y, e.z];
      });
    });
```

가속도계 센서에 대한 이벤트 콜백을 만들어서, double형인 리스트로 accelerometer라는 변수에 x축, y축, z축 값을 넣어줍니다.

setState에 의하여 값이 새로 들어가면 화면이 다시 그려지게 됩니다.

```dart
    gyroscopeEvents.listen((GyroscopeEvent e) {
      setState(() {
        gyroscope = <double>[e.x, e.y, e.z];
      });
    });
```

자이로스코프 센서에 대한 이벤트 콜백을 만들어서, double형인 리스트로 gyroscope라는 변수에 x축, y축, z축 값을 넣어줍니다.

setState에 의하여 값이 새로 들어가면 화면이 다시 그려지게 됩니다.

```dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
```

Center 생성자에 의해 값들이 가운데 정렬을 하게 됩니다.

```dart
          children: <Widget>[
            Text(
              accelerometer.toString(),
            ),
            Text(
              gyroscope.toString(),
            ),
          ],
        ),
      ),
    );
  }
}
```

Text 위젯들이 accelerometer와 gyroscope를 나란히 배치하고 double형 리스트를 문자열로 변환해줍니다.

이제 앱을 빌드하고 구동하면 센서의 값이 변화하는 것을 앱 내부에서 볼 수 있습니다.
