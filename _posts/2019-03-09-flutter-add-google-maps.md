---
layout: post
title: flutter 구글 지도 추가하기
---

오늘은 flutter로 구글 지도를 첨부할 수 있는 google_maps_flutter 패키지에 대하여 알아보려 합니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱으로 테스트할 장치를 준비해야 합니다.

준비했으면, 미리 기기 또는 에뮬레이터를 ide에 연결해줍니다.

## pubspec.yaml 작성하기

```yaml
dependencies:
  flutter:
    sdk: flutter
  google_maps_flutter:
```

flutter 앱에 구글 지도를 첨부하려면 google_maps_flutter 패키지를 pubspec.yaml에 작성해줍니다.

## api key

[해당 링크](https://developers.google.com/maps/documentation/android-sdk/signup)에서 Get an API key를 따라하여 google maps api 키를 미리 받아둡니다.

api 키를 발급받지 않고 진행할 경우, 지도가 정상적으로 출력되지 않습니다.

## android

안드로이드와 같은 경우에는 아래와 같이 AndroidManifest.xml 파일에 입력합니다.

```xml
<meta-data android:name="com.google.android.geo.API_KEY"
android:value="api 키 넣는 위치"/>
```

application 범위에서 존재하는 meta-data 속성에 미리 준비해둔 자신의 google maps api 키를 넣어주면 됩니다.

## ios

ios에서 지도를 첨부하려면, AppDelegate.m 혹은(AppDelegate.swift) 파일에 api 키를 넣어야 합니다.

```swift
GMSServices.provideAPIKey("YOUR KEY HERE")
```

[해당 링크](https://pub.dartlang.org/packages/google_maps_flutter#ios)에 자세히 설명되어있습니다.

## main.dart 앱 코드 작성하기

```dart
import 'package:flutter/material.dart';
import 'package:google_maps_flutter/google_maps_flutter.dart';
import 'dart:async';
```

앱에 머티리얼 위젯을 추가할 수 있는 material 패키지와 구글 지도를 넣을 google_maps_flutter 패키지를 가져옵니다.

Future 오브젝트를 생성하면서 값 또는 오류를 받아주는 Completer를 코드에서 사용하기 위해 async 패키지도 가져옵니다.

```dart
void main() => runApp(MyApp());
```

앱을 MyApp으로 구동합니다.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter google maps',
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
  Completer<GoogleMapController> _controller = Completer();
```

GoogleMap에 대한 Completer 컨트롤러를 만들어 줍니다.

```dart
  static final gwanghwamun = CameraPosition(
    target: LatLng(37.575929, 126.976849),
    zoom: 3.0,
  );
```

CameraPosition의 타켓으로 위도와 경도를 설정해두고, 줌 값도 설정합니다.

이 코드에서는 광화문의 위치를 보여주려 작성했습니다.

```dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: GoogleMap(
          mapType: MapType.hybrid,
          initialCameraPosition: gwanghwamun,
          onMapCreated: (GoogleMapController controller) {
            _controller.complete(controller);
          },
```

GoogleMap 위젯을 사용하여 맵의 타입과 처음 맵이 보여줄 위치를 설정하고, onMapCreated 콜백으로 컨트롤러를 제어합니다.

```dart
          compassEnabled: true,
          zoomGesturesEnabled: true,
          rotateGesturesEnabled: true,
          scrollGesturesEnabled: true,
          tiltGesturesEnabled: true,
        ),
      ),
    );
  }
}
```

각종 속성은 bool 값으로 제어할 수 있습니다.
