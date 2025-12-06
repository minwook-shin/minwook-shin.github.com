---
layout: post
title: flutter로 그리드 리스트 구현하기
---

오늘은 flutter의 ListView 속성 중에서 scrollDirection을 이용하여 가로 스크롤이 되는 리스트 앱을 만들어보려 합니다.

이 포스팅을 따라 오신다면 여러분도 가로 스크롤이 되는 리스트 뷰 앱을 안드로이드, ios 앱으로 동시에 만들어 볼 수 있습니다.

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
    title: 'Grid List', 
    home: Scaffold(body: MyApp())));
}
```

처음 앱이 실행될 때 앱의 타이틀과 메인스크린 위젯을 출력합니다.

```dart
class MyApp extends StatelessWidget {
  final items = List.generate(
      100,
      (i) => Text(
            i.toString(),
          ));
```

List.generate로 위젯들을 모아둔 리스트를 만들어줍니다.

```dart
  @override
  Widget build(BuildContext context) {
    return GridView.count(
```

MyApp이라는 StatelessWidget 클래스에서 그리드뷰를 구성합니다.

```dart
      crossAxisCount: 5,
      children: items,
```

한 줄에 5개가 나열되도록 속성에 값을 넣고, 미리 만들어둔 위젯들도 넣어줍니다.

```dart
      mainAxisSpacing: 30.0,
      crossAxisSpacing: 30.0,
      reverse: false,
    );
  }
}
```

항목별로 가로 세로 간격을 설정할 수 있고, reverse를 boolean 속성으로 그리드 리스트를 거꾸로 나열할 수도 있습니다.