---
layout: post
title: flutter로 커스텀 버튼 만들기
---

오늘은 flutter의 GestureDetector를 이용하여 커스텀 버튼 앱을 만들어보려 합니다.

이 포스팅을 따라 오신다면 여러분도 커스텀 버튼 앱을 안드로이드, ios 앱으로 동시에 만들어 볼 수 있습니다.

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
    title: 'Gesture',
    home: Scaffold(
```

처음 앱이 실행될 때 앱의 타이틀과 메인스크린 위젯을 출력합니다.

```dart
      body: Center(child:  GestureDetectorButton()),
    ),
  ));
}
```

GestureDetectorButton이라는 StatelessWidget 클래스를 만들어서 메인스크린 위젯의 가운데에 커스텀 버튼을 출력합니다.

```dart
class  GestureDetectorButton extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: () {
        print("탭");
      },
```

GestureDetector 위젯을 사용하고 onTap 콜백을 작성하면 해당 영역을 탭할 시에 해당 작업이 수행됩니다.

위 코드에서는 버튼을 누르면 콘솔에 문자열이 출력되게 됩니다.

```dart
      child: Container(
        padding: EdgeInsets.all(100.0),
        child: Text('버튼'),
```

GestureDetector 위젯의 하위에는 컨테이너로 모양을 잡아줍니다.

```dart
        decoration: BoxDecoration(
          color: Colors.amber,
          borderRadius: BorderRadius.circular(100.0),
        ),
      ),
    );
  }
}
```

BoxDecoration으로 색상을 지정할 수 있으며, 반경을 둥글게 설정할 수도 있습니다.