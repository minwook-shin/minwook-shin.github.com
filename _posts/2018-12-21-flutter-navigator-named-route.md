---
layout: post
title: flutter로 이름있는 라우터를 사용하여 멀티 스크린 앱 만들어보기
---

오늘도 어제처럼 flutter에서 이름있는 라우터를 사용하여 새로운 화면으로 넘어가는 앱을 만들어보려 합니다.

이 포스팅을 따라 오신다면 여러분도 같은 화면 구성이라도 각각 다른 기능을 할 수 있는 안드로이드, ios 앱을 동시에 만들어 볼 수 있습니다.

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
    title: 'Named Routes Demo',
    initialRoute: '/',
    routes: {
      '/': (context) => FirstScreen(),
      '/second': (context) => SecondScreen(),
    },
  ));
}
```

이전 포스팅에서는 바로 위젯으로 넘겼다면, 이번에는 루트를 지정하여 최초에 출력될 화면을 지정하고 각각의 화면을 구성하는 위젯들을 루트와 매칭시킬 수 있습니다.

```dart
class FirstScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('메인 화면'),
      ),
      body: Center(
        child: OutlineButton(
          child: Text('다음 화면'),
          onPressed: () {
            Navigator.pushNamed(context, '/second');
          },
        ),
      ),
    );
  }
}
```

그럼 StatelessWidget 타입의 첫번째 메인 화면을 작성해보려 합니다.

빌드 메소드를 오버라이딩하여 앱 바를 그리고, 본문에 OutlineButton도 그려줍니다.

onPressed 속성을 통해 OutlineButton를 누르면 Navigator.pushNamed로 메인의 routes에서 정해준 루트를 찾아 스택에 쌓아줍니다.

스택에 쌓았으므로, 이제 두번째 화면이 출력됩니다.

```dart
class SecondScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("열린 페이지"),
      ),
      body: Center(
        child: OutlineButton(
          onPressed: () {
            Navigator.pop(context);
          },
          child: Text('뒤로가기'),
        ),
      ),
    );
  }
}
```

두번째 화면 본문에 다시 버튼을 그려주고 onPressed 속성에 Navigator.pop을 부여하여 버튼이 눌리면 스택에서 라우터를 빼냅니다.

```dart
class FirstScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('메인 화면'),
      ),
      body: Center(
        child: OutlineButton(
          child: Text('다음 화면'),
          onPressed: () {
            Navigator.pushNamed(context, '/');
          },
        ),
      ),
    );
  }
}
```

위와 같이 / 로 작성하면 해당 이름을 가진 FirstScreen이 계속 스택에 쌓이면서 화면이 그려지고, 앱 바의 뒤로가기를 눌러 스택에서 pop하면 하나씩 화면이 뒤로 가집니다.

이제 앱을 핫 리로드하면 버튼으로 새로운 화면을 push하면서 보여주고, pop하면서 지우는 것이 가능해집니다.