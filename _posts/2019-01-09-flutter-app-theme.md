---
layout: post
title: flutter로 앱 테마 지정해보기
---

오늘은 flutter로 앱 테마를 지정하여 만들어보려 합니다.

이 포스팅을 따라 오신다면 여러분도 앱 테마를 지정하여 안드로이드, ios 앱을 동시에 만들어 볼 수 있습니다.

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
    title: "app theme",
    home: MyApp(),
```

처음 앱이 실행될 때 앱의 타이틀과 메인스크린 위젯을 출력합니다.

```dart
    theme: ThemeData(
      brightness: Brightness.light,
      primaryColor: Colors.blue,
      accentColor: Colors.black12,
      textTheme: TextTheme(
          headline: TextStyle(fontSize: 72.0, fontWeight: FontWeight.bold),
          title: TextStyle(fontSize: 36.0, fontStyle: FontStyle.italic),
          body1: TextStyle(fontSize: 14.0),
          display1: TextStyle(fontSize: 50.0)),
    ),
  ));
}
```

ThemeData를 MaterialApp 생성자에 제공할 수 있습니다.

앱의 테마를 지정하여 앱 내부에서 사용하는 디자인을 일괄적으로 적용할 수 있습니다.

위 코드에 나온대로 primaryColor 혹은 accentColor를 지정할 수 있고, 버튼이나 슬라이스의 색상들도 일괄적으로 변경할 수도 있습니다.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
          child: ListView(
        children: [
          Container(
            color: Theme.of(context).accentColor,
            child: Text(
              '헤드라인',
              style: Theme.of(context).textTheme.headline,
            ),
          ),
```

이제 Theme.of(context) 메소드를 통해서 앱 테마에 접근할 수 있습니다.

Theme.of(context) 메소드는 위젯 트리에서 가까운 앱 테마를 찾아 반환해줍니다.

위 코드는 accentColor와 텍스트 테마인 headline을 가져와서 텍스트 스타일을 정하게 됩니다.

```dart
          Container(
            child: Text(
              '타이틀',
              style: Theme.of(context).textTheme.title,
            ),
          ),
```

텍스트 테마인 title을 가져와서 텍스트 스타일을 정하게 됩니다.

```dart
          Container(
            child: Text(
              '바디',
              style: Theme.of(context).textTheme.body1,
            ),
          ),
```

텍스트 테마인 body1을 가져와서 텍스트 스타일을 정하게 됩니다.

```dart
          Container(
            child: Text(
              '디스플레이',
              style: Theme.of(context).textTheme.display1,
            ),
          ),
        ],
      )),
    );
  }
}
```

텍스트 테마인 display1을 가져와서 텍스트 스타일을 정하게 됩니다.