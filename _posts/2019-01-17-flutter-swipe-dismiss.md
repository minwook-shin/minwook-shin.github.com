---
layout: post
title: flutter로 밀어서 삭제할 수 있는 리스트 만들기
---

오늘은 flutter의 Dismissible을 이용하여 특정 항목을 밀어서 삭제할 수 있는 리스트 앱을 만들어보려 합니다.

이 포스팅을 따라 오신다면 여러분도 특정 항목을 밀어서 없앨 수 있는 앱을 안드로이드, ios 앱으로 동시에 만들어 볼 수 있습니다.

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
      title: 'Swipe Dismiss',
      home: Scaffold(body:MyApp())));
}
```

처음 앱이 실행될 때 앱의 타이틀과 메인스크린 위젯을 출력합니다.

```dart
class MyApp extends StatefulWidget {
  @override
  MyAppState createState() => MyAppState();
}
```

이제 StatefulWidget으로 버튼, 슬라이더, 체크 박스와 같이 사용자와 상호작용하여 위젯이 변화하는 것을 묶어줍니다.

createState 메소드를 오버라이딩하여 MyAppState 객체를 생성해줍니다.

```dart
class MyAppState extends State<MyApp> {
  final items = List.generate(10, (i) => "리스트 ${i + 1}");
```

리스트는 generate 메소드를 이용하여 1부터 10까지의 리스트 항목을 만들어줍니다.

```dart
  @override
  Widget build(BuildContext context) {
    return  ListView.builder(
          itemCount: items.length,
          itemBuilder: (context, index) {
            return Dismissible(
              key: Key(items[index]),
              onDismissed: (direction) {
                setState(() {
                  items.removeAt(index);
                });
              },
```

리스트뷰 빌더로 각 항목을 Dismissible 위젯으로 감싸줍니다.

key로 각 항목들을 구별하는데에 사용됩니다.

이제 각 항목들은 스와이프하면 onDismissed 콜백이 활성화되게 됩니다.

이 코드에서는 onDismissed 콜백을 수행하면 리스트에서 해당 위치의 항목이 제거되면서 setState 메소드에 의해 화면이 다시 그려지게 됩니다.

```dart
              child: ListTile(title: Text(items[index])),
              background: Container(
                  alignment: Alignment.center,
                  color: Colors.red,
                  child: Text(
                    "삭제",
                    style: TextStyle(fontSize: 50),
                  )),
            );
          },
        );
  }
}
```

background 속성을 채우면 스와이프할 때에 뒤에서 보이는 화면을 꾸밀 수 있습니다.

위 코드에서는 빨간색의 배경에 가운데 정렬이 된 "삭제"라는 문자열을 출력한 상태입니다.