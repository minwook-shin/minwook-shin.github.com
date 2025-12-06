---
layout: post
title: flutter image picker 사용하기
---

오늘은 flutter로 image picker를 사용할 수 있는 image_picker 패키지에 대하여 알아보려 합니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱으로 테스트할 장치를 준비해야 합니다.

준비했으면, 미리 기기 또는 에뮬레이터를 ide에 연결해줍니다.

## pubspec.yaml 작성하기

```yaml
dependencies:
  flutter:
    sdk: flutter
  image_picker:
```

flutter 앱에 이미지를 촬영해서 첨부하거나 갤러리에서 이미지를 가져오려면 image_picker 패키지를 pubspec.yaml에 작성해줍니다.

## ios

안드로이드와 같은 경우에는 별도의 작업을 할 필요가 없으나, ios는 카메라 권한 설명을 첨부해야 합니다.

Runner 폴더의 Info.plist 파일에 NSPhotoLibraryUsageDescription, NSCameraUsageDescription, NSMicrophoneUsageDescriptiond를 작성해주어야 합니다.

```xml
	<key>NSCameraUsageDescription</key>
	<string>Used to demonstrate image picker plugin</string>
	<key>NSMicrophoneUsageDescription</key>
	<string>Used to capture audio for image picker plugin</string>
	<key>NSPhotoLibraryUsageDescription</key>
	<string>Used to demonstrate image picker plugin</string>
```

## main.dart 앱 코드 작성하기

```dart
import 'package:flutter/material.dart';
import 'package:image_picker/image_picker.dart';
import 'dart:io';
```

앱에 머티리얼 위젯을 추가할 수 있는 material 패키지와 이미지를 갤러리와 카메라에서 가져올 image_picker 패키지를 가져옵니다.

파일을 담을 변수 타입을 쓰기 위해서 io 패키지도 가져옵니다.

```dart
void main() => runApp(MyApp());
```

앱을 MyApp으로 구동합니다.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter image picker',
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
  File _image;
```

이미지 파일을 담을 변수를 만들어줍니다.

```dart
  getGalleryImage() async {
    var image = await ImagePicker.pickImage(source: ImageSource.gallery);
    setState(() {
      _image = image;
    });
  }
```

getGalleryImage라는 비동기 함수는 ImagePicker 패키지의 pickImage에 의해서 기기의 갤러리에서 사진을 가져옵니다.

image 변수를 \_image에 옮기면서 setState로 화면이 다시 새로 고쳐집니다.

```dart
  getCameraImage() async {
    var image = await ImagePicker.pickImage(source: ImageSource.camera);
    setState(() {
      _image = image;
    });
  }
```

getCameraImage라는 비동기 함수는 ImagePicker 패키지의 pickImage에 의해서 기기의 카메라에서 사진을 촬영하고 바로 앱으로 가져옵니다.

image 변수를 \_image에 옮기면서 setState로 화면이 다시 새로 고쳐집니다.

```dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: _image == null ? Text('No image') : Image.file(_image),
      ),
```

이미지가 없을 때는 문자열이 출력되고, 이미지가 존재하면 이미지 파일을 Image.file로 그립니다.

```dart
      floatingActionButton: Column(
        mainAxisAlignment: MainAxisAlignment.end,
        children: <Widget>[
          FloatingActionButton(
            onPressed: getGalleryImage,
            tooltip: 'getGalleryImage',
            child: Icon(Icons.photo_library),
          ),
```

photo_library 아이콘 모양의 floating 버튼을 누르면 getGalleryImage를 수행해서 갤러리의 사진을 가져오게 합니다.

```dart
          FloatingActionButton(
            onPressed: getCameraImage,
            tooltip: 'getCameraImage',
            child: Icon(Icons.photo_camera),
          ),
        ],
      ),
    );
  }
}
```

photo_camera 아이콘 모양의 floating 버튼을 누르면 getCameraImage를 수행해서 카메라로 직접 사진을 촬영하게 합니다.
