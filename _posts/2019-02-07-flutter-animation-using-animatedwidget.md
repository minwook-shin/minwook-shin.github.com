---
layout: post
title: flutter 애니메이션을 AnimatedWidget으로 적용해보기
---

오늘은 어제 소개한 flutter 위젯에서 AnimatedWidget으로 애니메이션 적용해보는 방법을 알아보려 합니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱으로 테스트할 장치를 준비해야 합니다.

준비했으면, 미리 기기 또는 에뮬레이터를 ide에 연결해줍니다.

## pubspec.yaml 작성하기

이번 포스팅에서는 따로 설정할 것이 없으므로 new project로 설정된 그대로 사용합니다.

## main.dart 앱 코드 작성하기

```dart
import 'package:flutter/material.dart';
```

머티리얼 디자인의 앱을 만들기 위해 material 패키지를 가져옵니다.

```dart
class Animated extends AnimatedWidget {
  Animated({Key key, @required Animation<double> animation})
      : super(key: key, listenable: animation);
```

이번 포스팅에서는 저번과 다르게 이벤트 리스너에서 setstate를 불러오지 않고, AnimatedWidget으로 listenable이 변화할 때마다 다시 생성하도록 코드를 작성합니다.

listenable은 메인에서 받은 animation 객체입니다.

```dart
  Widget build(BuildContext context) {
    final Animation<double> animation = listenable;
    return Center(
      child: Container(
          height: Tween(begin: 0.0, end: 200.0).evaluate(animation),
          width: Tween(begin: 0.0, end: 200.0).evaluate(animation),
          child: CircularProgressIndicator()),
    );
  }
}
```

객체의 인자로 받은 listenable을 animation로 쓰면서 Container를 반환합니다.

애니메이션을 적용할 컨테이너의 height와 width를 설정하고 하위 위젯으로 CircularProgressIndicator를 넣어줍니다.

```dart
void main() => runApp(MyApp());
```

MyApp 객체를 앱이 구동할 때 실행합니다.

```dart
class MyApp extends StatefulWidget {
  @override
  _MyHomePageState createState() => _MyHomePageState();
}
```

StatefulWidget으로 버튼, 슬라이더, 체크 박스처럼 사용자와 상호작용하여 위젯이 변화하는 것을 묶어줍니다.

createState 메소드를 오버라이딩하여 _MyHomePageState 객체를 생성해줍니다.

```dart
class _MyHomePageState extends State<MyApp> with SingleTickerProviderStateMixin {
  AnimationController controller;
  Animation animation;
```

vsync를 사용하기 위해 클래스에 SingleTickerProviderStateMixin를 추가하고, AnimationController와 Animation 객체를 생성해줍니다.

```dart
  @override
  void initState() {
    super.initState();
    controller =
        AnimationController(duration: Duration(seconds: 5), vsync: this);
    controller.forward();
  }
```

위젯이 그려질 때에 initState가 수행되면서 controller에 애니메이션을 관리하는 AnimationController를 부여합니다.

forward 메소드로 애니메이션을 시작합니다.

```dart
  @override
  void dispose() {
    super.dispose();
    controller.dispose();
  }
```

State가 끝날 때에 수행되는 dispose로 인해 controller도 종료됩니다.

```dart
  @override
  Widget build(BuildContext context) {
    animation = CurvedAnimation(parent: controller, curve: Curves.bounceInOut);
```

애니메이션의 진행을 관리하는 CurvedAnimation로 Animation에 부여합니다.

```dart
    animation.addStatusListener((status) {
      if (status == AnimationStatus.completed) {
        controller.reverse();
      } else if (status == AnimationStatus.dismissed) {
        controller.forward();
      }
    });
```

애니메이션이 시작되고, 중지되는 상태를 감지하는 이벤트 리스너를 addStatusListener으로 추가해서 이벤트가 완료되면 역순으로 재생하거나, 다시 재생하게 할 수 있습니다.

```dart
    return MaterialApp(
      home: Scaffold(body: Animated(animation: animation)),
    );
  }
}
```

머티리얼 디자인의 앱 틀을 만들기 위해 MaterialApp과 Scaffold로 감싼 다음에 AnimatedWidget 객체를 불러옵니다.

앱을 빌드하면 서클 진행바가 움직이면서 애니메이션이 진행됩니다.