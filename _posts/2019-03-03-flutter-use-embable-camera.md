---
layout: post
title: flutter로 내장된 카메라 사용하기
---

오늘은 flutter로 앱이 실행되고 있는 기기의 카메라를 사용해보려 합니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱으로 테스트할 장치를 준비해야 합니다.

준비했으면, 미리 기기 또는 에뮬레이터를 ide에 연결해줍니다.

## pubspec.yaml 작성하기

```yaml
dependencies:
  flutter:
    sdk: flutter
  camera:
```

flutter에서 카메라를 사용하려면 camera 패키지를 pubspec.yaml에 작성해줍니다.

## android

안드로이드에서 해당 패키지를 사용하려면 build.gradle 파일을 열고 약간의 작업이 필요합니다.

```gradle
defaultConfig {
        // TODO: Specify your own unique Application ID (https://developer.android.com/studio/build/application-id.html).
        applicationId "com.example.flutter_camera_test"
        minSdkVersion 21
        targetSdkVersion 28
        versionCode flutterVersionCode.toInteger()
        versionName flutterVersionName
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    }
```

minSdkVersion가 기본적으로 16이라고 적혀있지만, 21로 설정해야 합니다.

## ios

ios/Runner/Info.plist 파일에서 카메라와 마이크로 폰의 키를 설정해야 합니다.

## main.dart 앱 코드 작성하기

```dart
import 'package:flutter/material.dart';
import 'package:camera/camera.dart';
```

앱에 머티리얼 위젯을 추가할 수 있는 material 패키지와 카메라를 사용할 수 있는 camera 패키지를 가져옵니다.

```dart
List<CameraDescription> cameras;
```

cameras의 목록을 만들 수 있는 변수를 선언합니다.

```dart
void main() async {
  cameras = await availableCameras();
  runApp(MyApp());
}
```

사용 가능한 카메라 목록에 추가하고 앱을 MyApp으로 구동합니다.

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
  CameraController controller;
```

카메라를 제어할 수 있는 카메라 컨트롤러를 추가합니다.

```dart
  @override
  void initState() {
    super.initState();
    controller = CameraController(cameras[0], ResolutionPreset.high);
    controller.initialize().then((_) {
      if (!mounted) {
        return;
      }
      setState(() {});
    });
  }
```

initState를 오버라이딩하여 카메라 목록의 첫번째 카메라를 CameraController에 추가하고, 초기화합니다.

초기화할 때에는 setState에 의해 화면을 그리게 됩니다.

```dart
  @override
  void dispose() {
    controller?.dispose();
    super.dispose();
  }
```

앱이 dispose될 때에는 컨트롤러도 처리합니다.

```dart
  @override
  Widget build(BuildContext context) {
    if (!controller.value.isInitialized) {
      return Container();
    }
```

만약 컨트롤러가 초기화되지 않았다면, Container만 화면을 그려줍니다.

```dart
    return Scaffold(
        body: AspectRatio(
            aspectRatio: controller.value.aspectRatio,
            child: CameraPreview(controller)),
        floatingActionButton: FloatingActionButton(
          child: Icon(Icons.camera),
          onPressed: () async {
            // controller.takePicture(path)
          },
        ));
  }
}
```

만약 컨트롤러가 초기화되있다면, 카메라의 화면을 보여주는 CameraPreview를 가로세로비를 관리하는 AspectRatio의 안에 넣어줍니다.

주석에 적힌대로 controller로 다양한 행동을 할 수 있습니다.
