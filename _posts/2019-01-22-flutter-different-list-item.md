---
layout: post
title: flutter로 서로 다른 위젯이 섞인 리스트 구현하기
---

오늘은 flutter의 조건문으로 여러 위젯들이 섞인 ListView 리스트 앱을 만들어보려 합니다.

이 포스팅을 따라 오신다면 여러분도 여러 위젯들이 혼용된 리스트 뷰 앱을 안드로이드, ios 앱으로 동시에 만들어 볼 수 있습니다.

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
    title: 'different List', 
    home: Scaffold(body: MyApp())
    ));
}
```

처음 앱이 실행될 때 앱의 타이틀과 메인스크린 위젯을 출력합니다.

```dart
class MyApp extends StatelessWidget {
  final List<Widget> items = List.generate(
    100,
    (i) => i % 3 == 0 ? Container() : Card(),
  );
```

List.generate로 위젯들을 모아둔 리스트를 만들어줍니다.

간단한 설명을 위해 삼항연산자를 통해 컨테이너와 카드 생성자를 규칙적으로 나열해줍니다.

```dart
  @override
  Widget build(BuildContext context) {
    return ListView.builder(
      itemCount: items.length,
```

ListView의 builder로 리스트를 구성해줍니다.

```dart
      itemBuilder: (context, index) {
        if (items[index] is Container) {
          return ListTile(title: Text(items[index].toString()));
```

itemBuilder에서 해당 리스트의 index가 Container 생성자라면 정해진 ListTile을 반환합니다.

```dart
        } else if (items[index] is Card) {
          return ListTile(title: Text(items[index].toString()));
        }
      },
    );
  }
}
```

해당 리스트의 index가 Card 생성자라면 정해진 ListTile을 반환합니다.