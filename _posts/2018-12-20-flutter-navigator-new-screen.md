---
layout: post
title: flutter로 멀티 스크린 앱 만들어보기
---

오늘은 flutter에서 네비게이터를 이용하여 새로운 화면으로 넘어가는 앱을 만들어보려 합니다.

이 포스팅을 따라 오신다면 여러분도 멀티스크린 안드로이드, ios 앱을 동시에 만들어 볼 수 있습니다.

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
    title: 'Navigation Basics',
    home: FirstScreen(),
  ));
}
```

이전 포스팅에서 보였던 home의 위치에 앱 바와 본문을 위젯들로 나열해두었다면, 이번에는 홈을 StatelessWidget으로 바로 넘겼습니다.

이로서 안드로이드에서 엑티비티라고 불리는 것이 flutter에서는 StatelessWidget임을 알 수 있습니다.

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
          child: Text('다음 페이지'),
          onPressed: () {
            Navigator.push(
              context,
              MaterialPageRoute(builder: (context) => SecondScreen()),
            );
          },
        ),
      ),
    );
  }
}
```

그럼 StatelessWidget 타입의 첫번째 메인 화면을 작성해보려 합니다.

빌드 메소드를 오버라이딩하여 앱 바를 그리고, 본문에 OutlineButton도 그려줍니다.

onPressed 속성을 통해 OutlineButton를 누르면 Navigator.push를 수행해서 라우터를 스택에 쌓습니다.

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

이제 앱을 핫 리로드하면 버튼으로 새로운 화면을 push하면서 보여주고, pop하면서 지우는 것이 가능해집니다.