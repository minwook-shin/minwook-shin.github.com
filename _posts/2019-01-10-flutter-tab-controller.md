---
layout: post
title: flutter로 탭 레이아웃 만들어보기
---

오늘은 flutter에서 DefaultTabController로 탭 레이아웃을 만들어보려 합니다.

이 포스팅을 따라 오신다면 여러분도 머티리얼 디자인가이드에 따르는 탭을 안드로이드, ios 앱을 동시에 만들어 볼 수 있습니다.

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
    home: DefaultTabController(
```

처음 앱이 실행될 때 탭을 출력하기 위해 DefaultTabController를 생성힙니다.

DefaultTabController를 만들면 TabController가  만들어지면서 모든 하위 위젯트리에서 사용할 수 있기 때문입니다.

```dart
      child: Scaffold(
        appBar: AppBar(
          title: Text('Tabs'),
          bottom: TabBar(
            tabs: [
              Tab(icon: Icon(Icons.apps),text: "앱"),
              Tab(icon: Icon(Icons.audiotrack),text: "오디오"),
              Tab(icon: Icon(Icons.settings),text: "설정"),
            ],
          ),
        ),
```

appBar에서 타이틀과 탭 바를 정의해줍니다.

한 탭에는 아이콘과 이름이 들어갈 수 있습니다.

TabBar의 tabs를 나열할수록 옆으로 넘길 수 있는 탭은 계속 증가합니다.

만약 수동으로 DefaultTabController가 아닌 TabController로 직접 생성하는 경우에는 TabBar에 TabController를 전달해야합니다.

```dart
        body: TabBarView(
          children: [
            Container(
              color: Colors.red,
            ),
            Container(
              color: Colors.green,
            ),
            Container(
              color: Colors.blue,
            ),
          ],
        ),
      ),
```

이제 body를 지정해야 탭마다 서로 다른 화면으로 넘어가집니다.

TabBarView를 이용하여 내용을 채워줍니다.

위 코드에서는 Container를 이용하여 색상으로 화면을 채웠습니다.

반드시 탭 바의 순서와 일치해야 합니다.

```dart
      length: 3,
    ),
  ));
}
```

마지막으로 탭의 갯수를 기입해야 합니다.

이와 같이 코드를 작성한 뒤에 빌드를 하면, 탭 레이아웃이 나타나며 좌우로 스크롤할 수 있습니다.