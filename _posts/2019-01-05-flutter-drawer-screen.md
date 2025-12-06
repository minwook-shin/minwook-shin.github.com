---
layout: post
title: flutter로 드로우 화면 만들어보기
---

오늘은 flutter로 drawer screen을 구현한 앱을 만들어보려 합니다.

이 포스팅을 따라 오신다면 여러분도 안드로이드에서 drawer 레이아웃처럼 동작할 수 있는 안드로이드, ios 앱을 동시에 만들어 볼 수 있습니다.

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
      title: 'Drawer',
      home: MyApp(),
    ));
```

처음 앱이 실행될 때 앱의 타이틀과 메인스크린 위젯을 출력합니다.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
```

drawer screen을 펼치기 위한 버튼이 포함된 앱 바를 생성해줍니다.

```dart
      drawer: Drawer(
        child: ListView(
          padding: EdgeInsets.zero,
```

drawer 속성으로 drawer screen을 만들어줍니다.

drawer screen에는 ListView로 나열되며, padding을 설정할 수도 있습니다.

```dart
          children: [
            DrawerHeader(
              child: Text('Header'),
              decoration: BoxDecoration(
                color: Colors.blue,
              ),
            ),
```

ListView의 내용에는 헤더와 리스트 타일이 위치할 수 있습니다.

decoration 속성으로 색상이나 그림자와 같은 옵션을 지정할 수 있습니다.

```dart
            ListTile(
              title: Text('Item'),
            ),
          ],
        ),
      ),
    );
  }
}
```

리스트 타일로 헤더 아래에 위치한 아이템을 구현할 수 있습니다.

위와 같이 작성하게 되면 안드로이드에서의 drawer layout과 동일하게 구현할 수 있습니다.