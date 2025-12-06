---
layout: post
title: flutter로 새로운 화면에서 데이터 넘겨받는 앱 만들어보기
---

오늘은 flutter로 버튼을 눌렀을 때에 기다렸다가 새로운 화면에서 데이터를 받아 스낵바로 출력하는 앱을 만들어보려 합니다.

이 포스팅을 따라 오신다면 여러분도 데이터들을 다른 화면에서 받는 기능을 할 수 있는 안드로이드, ios 앱을 동시에 만들어 볼 수 있습니다.

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
    title: 'Returning Data',
    home: HomeScreen(),
  ));
}
```

처음 앱이 실행될 때 앱의 타이틀과 메인스크린 위젯을 출력합니다.

```dart
class HomeScreen extends StatelessWidget {
  final _scaffold = GlobalKey<ScaffoldState>();
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      key: _scaffold,
      body: Center(
        child: OutlineButton(
          onPressed: () {
            _navigateAndDisplaySelection(context);
          },
          child: Text('선택하기'),
        ),
      ),
    );
  }
```

BuildContext으로서 다른 객체에 접근할 수 있는 글로벌 키 변수를 만들어주고, 버튼을 하나 만들어서 누르면 새로운 화면이 출력되도록 작성합니다.

이때 만들어진 글로벌 키는 위젯 트리에서 부모로 접근해서 자식에게 전달할 수 있으므로 스낵바를 만들때 사용됩니다.

```dart
  _navigateAndDisplaySelection(BuildContext context) async {
    final result = await Navigator.push(
      context,
      MaterialPageRoute(builder: (context) => SelectionScreen()),
    );
```

SelectionScreen을 화면에 띄우고, pop이 될 때까지 기다립니다.

```dart
    _scaffold.currentState
      ..removeCurrentSnackBar()
      ..showSnackBar(SnackBar(content: Text("$result")));
  }
}
```

만약 pop이 수행 되어서 push가 종료되어 result로 반환된다면 새로운 화면에서 선택되어 돌아온 문자열이므로, 이를 스낵바로 출력합니다.

아까 받은 글로벌 키로 부모의 자식관계를 받아 현재 띄워진 다른 스낵바를 지우고, 현재 문자열에 맞는 새로운 스낵바를 띄웁니다.

```dart
class SelectionScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Column(
        children: <Widget>[
          OutlineButton(
            onPressed: () {
              Navigator.pop(context, '예');
            },
            child: Text('예'),
          ),
          OutlineButton(
            onPressed: () {
              Navigator.pop(context, '아니오');
            },
            child: Text('아니오'),
          ),
        ],
      ),
    );
  }
}
```

새로 출력되는 화면에서는 위와 같이 버튼 두개가 부여되어 있으며, 이 버튼들은 서로 다른 pop 반환값을 가지고 있습니다.

만약 특정 버튼을 누르게되면 메인 화면으로 돌아가서 push 메소드에 반환값이 들어오게 됩니다.

최종적으로 반환값이 들어오게 되면 비동기 함수에 의해 응답되어 스낵바가 출력되게 됩니다.

이제 앱을 핫 리로드하면 정상적으로 앱이 빌드됨을 확인할 수 있습니다.