---
layout: post
title: flutter로 인터넷에서 받은 이미지 가져오기
---

오늘은 flutter의 Image.network를 이용하여 인터넷에서 이미지를 가져와서 출력하는 앱을 만들어보려 합니다.

이 포스팅을 따라 오신다면 여러분도 특정 이미지를 인터넷에서 가져와서 출력하는 앱을 안드로이드, ios 앱으로 동시에 만들어 볼 수 있습니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱을 구동할 장치를 준비해야 합니다.

준비했으면, 미리 기기를 ide에 연결해줍니다.

## pubspec.yaml 작성하기

이번 포스팅에서는 따로 설정할 것이 없으므로 new project로 설정된 그대로 사용합니다.

## main.dart 작성하기

이제 메인 코드를 작성해보아야 합니다.

```dart
import 'package:flutter/material.dart';
```

flutter/material 패키지를 사용하겠다는 것이지만, 대부분의 앱에서 반드시 가져와야 하는 필수 패키지입니다.

```dart
void main() {
  runApp(MaterialApp(
      title: 'Images from internet', 
      home: Scaffold(body: MyApp())));
}
```

처음 앱이 실행될 때 앱의 타이틀과 메인스크린 위젯을 출력합니다.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return ListView(
```

MyApp이라는 StatelessWidget 클래스에서 리스트뷰를 구성합니다.

```dart
      children: <Widget>[
        Image.network(
          'https://picsum.photos/250?image=9',
          fit: BoxFit.fill,
        ),
```

리스트뷰에는 위젯들을 나열할 수 있으므로, 네트워크에서 가져오는 이미지를 출력하는 위젯을 배치합니다.

Image.network로 url에서 이미지를 가져올 수 있습니다.

```dart
        Image.network(
          'https://github.com/flutter/plugins/raw/master/packages/video_player/doc/demo_ipod.gif?raw=true',
          fit: BoxFit.fill,
        ),
      ],
    );
  }
}
```

Image.network로 움직이는 gif 파일도 불러올 수 있습니다.