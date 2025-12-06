---
layout: post
title: flutter로 ripple 효과있는 버튼 만들기
---

오늘은 flutter의 InkWell을 이용하여 버튼에 ripple 효과를 주는 앱을 만들어보려 합니다.

이 포스팅을 따라 오신다면 여러분도 잉크 퍼지는 효과를 가진 ripple 버튼 앱을 안드로이드, ios 앱으로 동시에 만들어 볼 수 있습니다.

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
    title: 'InkWell Demo',
    home: Scaffold(
```

처음 앱이 실행될 때 앱의 타이틀과 메인스크린 위젯을 출력합니다.

```dart
      body: Center(child: InkWellButton()),
    ),
  ));
}
```

InkWellButton이라는 StatelessWidget 클래스를 만들어서 메인스크린 위젯의 가운데에 버튼을 출력합니다.

```dart
class InkWellButton extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return InkWell(
      onTap: () {
        print("탭");
      },
      child: Container(
        padding: EdgeInsets.all(50.0),
        child: Text('button'),
      ),
    );
  }
}
```

InkWell을 사용하여 머티리얼 디자인 가이드를 따르는 ripple 효과 버튼을 만들 수 있습니다.

위 코드에서는 onTap 속성을 이용하여 버튼을 누를 때마다 콘솔에 문자열로 출력이 됩니다.