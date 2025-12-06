---
layout: post
title: flutter로 화면 회전 감지하는 앱 만들어보기
---

오늘은 flutter로 화면 회전을 감지하는 앱을 만들어보려 합니다.

이 포스팅을 따라 오신다면 여러분도 화면 회전을 감지하여 앱의 화면을 다르게 배치할 수 있는 안드로이드, ios 앱을 동시에 만들어 볼 수 있습니다.

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
    title: "orientation app",
    home: MyApp(),
  ));
}
```

처음 앱이 실행될 때 앱의 타이틀과 메인스크린 위젯을 출력합니다.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(body: Center(
```

MyApp이라는 클래스에서 작업합니다.

```dart
      child: OrientationBuilder(builder: (context, orientation) {
        return Text(orientation == Orientation.portrait ? "세로" : "가로",
            style: TextStyle(fontSize: 100));
      }),
    ));
  }
}
```

OrientationBuilder 위젯은 부모 위젯에서 사용할 수 있는 높이와 너비를 비교하여 현재 보고있는 방향을 계산해줍니다.
만약 계산된 내용이 달라지면 다시 화면을 그립니다.

그리하여 위 코드는 OrientationBuilder 위젯을 이용하여 Orientation.portrait의 결과에 따라 출력되는 내용이 달라지게 됩니다.