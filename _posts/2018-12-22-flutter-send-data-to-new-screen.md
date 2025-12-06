---
layout: post
title: flutter로 새로운 화면에 데이터 넘기는 앱 만들어보기
---

오늘은 flutter로 새로운 화면으로 넘어가면서 데이터도 같이 넘기는 앱을 만들어보려 합니다.

이 포스팅을 따라 오신다면 여러분도 데이터들을 다른 화면에 넘기는 기능을 할 수 있는 안드로이드, ios 앱을 동시에 만들어 볼 수 있습니다.

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
class Thread {
  Thread(this.title, this.body);

  final title;
  final body;
}
```

제목과 본문을 나타내는 클래스를 작성합니다.

생성자를 만들어주면서 현재 인스턴스를 가르켜줍니다.

```dart
void main() {
  runApp(MaterialApp(
    title: 'Passing Data',
    home: MainScreen(
      array: List.generate(
        10,
        (i) => Thread(
              '$i',
              '$i 번째 리스트입니다.',
            ),
      ),
    ),
  ));
}
```

처음 앱이 실행될 때 앱의 타이틀과 메인스크린 위젯을 출력합니다.

아래 코드에서 보시다시피 MainScreen은 인자가 요구되기 때문에 Thread 타입의 리스트를 0부터 9까지 생성해줍니다.

```dart
class MainScreen extends StatelessWidget {
  final List<Thread> array;

  MainScreen({Key key, @required this.array}) : super(key: key);
```

인스턴스를 생성하면 인자로 들어온 리스트가 array로 저장되게 됩니다.

```dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: ListView.builder(
        itemCount: array.length,
        itemBuilder: (context, index) {
          return ListTile(
            title: Text(array[index].title),
            onLongPress: () {
              Navigator.push(
                context,
                MaterialPageRoute(
                  builder: (context) => DetailScreen(t: array[index]),
                ),
              );
            },
          );
        },
      ),
    );
  }
}
```

ListView를 이용하여 보여주기 때문에 위와 같이 작성합니다.

onLongPress 속성을 사용하여 리스트의 항목을 길게 누르면 
상세 페이지로 넘어가도록 작성합니다.

이 때에도 인자로 리스트의 Thread 한 세트를 넣어줍니다.

```dart
class DetailScreen extends StatelessWidget {
  final t;

  DetailScreen({Key key, @required this.t}) : super(key: key);
```

MainScreen와 같이 인자를 필요하다고 선언해줍니다.

```dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("${t.title}"),
      ),
      body: Text('${t.body}'),
    );
  }
}
```

인자에서 데이터를 가져왔으니 앱 바와 본문에 사용해줍니다.

이제 앱을 핫 리로드하면 리스트에 0부터 9까지 출력되며, 길게 누를 경우에는 다음 페이지로 넘어가며 문자열이 출력됩니다.