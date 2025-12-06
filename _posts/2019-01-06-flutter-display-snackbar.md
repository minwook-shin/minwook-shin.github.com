---
layout: post
title: flutter로 스낵바 출력하는 앱 만들어보기
---

오늘은 flutter로 snackbar 구현 앱을 만들어보려 합니다.

이 포스팅을 따라 오신다면 여러분도 안드로이드에서의 스낵바처럼 동작할 수 있는 안드로이드, ios 앱을 동시에 만들어 볼 수 있습니다.

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
void main() => runApp(MaterialApp(
      title: 'SnackBar',
      home: Scaffold(
        body: MyApp(),
      ),
    ));
```

처음 앱이 실행될 때 앱의 타이틀과 메인스크린 위젯을 출력합니다.

```dart
class MyApp extends StatelessWidget {
  final snackBar = SnackBar(
    content: Text('SnackBar!'),
    action: SnackBarAction(
      label: 'Undo',
      onPressed: () {
      },
    ),
  );
```

스낵바를 만들어서 변수에 담아줍니다.

스낵바는 들어가는 텍스트, 행동, 배경 색상, 애니메이션 등을 지정할 수 있습니다.

```dart
  @override
  Widget build(BuildContext context) {
    return 
      OutlineButton(
        onPressed: () {
          Scaffold.of(context).showSnackBar(snackBar);
        },
        child: Text('SnackBar'),
      );
  }
}
```

버튼을 그린 다음, 버튼을 누르면 스낵바가 나오도록 미리 만들어둔 스낵바 변수를 위젯 트리에 존재하는 Scaffold로 사용해줍니다.

이제 앱을 빌드하면 버튼이 출력되고, 이 버튼을 누르게되면 하단에서 스낵바가 출력됨을 보실 수 있습니다.