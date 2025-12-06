---
layout: post
title: flutter로 간단한 레이아웃 앱 만들어보기
---

오늘은 flutter 1.0으로 간단한 레이아웃을 지닌 앱을 만들어보려합니다.

이 포스팅을 따라 오신다면 여러분도 간단한 레이아웃 구조를 가진 안드로이드, ios 앱을 동시에 만들어 볼 수 있습니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱을 구동할 장치를 준비해야 합니다.

준비했으면, 미리 기기를 ide에 연결해줍니다.

## 이미지 파일 준비하기

https://raw.githubusercontent.com/flutter/website/master/src/_includes/code/layout/lakes/images/lake.jpg

앱에 사용될 이미지를 준비해줍니다.

이 이미지는 flutter 튜토리얼 사이트의 Building Layouts에 포함된 이미지로서 BSD License가 적용됩니다.

> code samples are licensed under the BSD License.

## pubspec.yaml 작성하기

우선 노드js의 npm처럼 패키지 매니저를 사용할 수 있습니다.

패키지의 이름이나 버전 그리고 의존성 패키지들을 관리할 수 있습니다.

https://pub.dartlang.org 에서 패키지를 조회하고 내려받을 수 있습니다.

```yaml
flutter:
  uses-material-design: true
  assets:
    - lake.jpg
```

기본적으로 새로운 프로젝트를 생성하면 이미지를 에셋에 등록하기 위해 위와 같이 등록시켜줍니다.

## main.dart 작성하기

이제 메인 코드를 작성해보아야 합니다.

```dart
import 'package:flutter/material.dart';
```

flutter/material 패키지를 사용하겠다는 것이지만, 대부분의 앱에서 반드시 가져와야 하는 필수 패키지입니다.

```dart
void main() => runApp(MyApp());
```

앱이 실행되면 main함수가 수행되면서 MyApp 객체를 실행하게 됩니다.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
```

MyApp은 StatelessWidget을 상속받아서 앱의 화면에 위젯을 그리는 담당을 합니다.

build 메소드를 오버라이딩하여 이 클래스에 포함된 위젯들을 반환합니다.

```dart
    Widget titleSection = Container(
      padding: EdgeInsets.all(30.0),
      child: Row(
        children: [
          Expanded(
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Container(
                  child: Text(
                    '제목',
                    style: TextStyle(
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                ),
                Text(
                  '부제목',
                ),
              ],
            ),
          ),
```

titleSection 이라는 위젯을 생성하면 아무 스타일도 적용되지 않은 상태이므로 상단에 위치합니다.

그리고, padding 과 같이 속성을 정의하여 스타일을 지정할 수 있습니다.

child로 하위의 구조를 작성하면서 Row, Column을 지정할 수 있습니다.

children으로 같은 레벨끼리 묶을 수 있습니다.

Expanded 위젯에 Column을 넣으면 행에 있는 남아 있는 모든 공간을 사용하도록 늘어나게 됩니다.



```dart
          Icon(
            Icons.star,
            color: Colors.red[500],
          ),
          Text('0'),
        ],
      ),
    );
```

Expanded 위젯과 같은 레벨에 아이콘을 배치합니다.

```dart
    Widget textSection = Container(
      padding: EdgeInsets.all(30.0),
      child: Text(
        '''
설명입니다.설명입니다.설명입니다.설명입니다.설명입니다.설명입니다.설명입니다.설명입니다.설명입니다.설명입니다.설명입니다.설명입니다.설명입니다.설명입니다.
        ''',
        softWrap: true,
      ),
    );
```

줄넘김이 가능한 텍스트를 컨테이너로 감싼 뒤에 textSection 위젯으로 만들어줍니다.

```dart
    return MaterialApp(
      title: 'Flutter Demo',
      home: Scaffold(
        appBar: AppBar(
          title: Text('Building UI'),
        ),
```

MyApp이 종료될 때에 MaterialApp을 반환합니다.

앱바의 이름을 바꿀 수 있습니다.

```dart
        body: ListView(
          children: [
            Image.asset(
              'lake.jpg',
              width: 600.0,
              height: 240.0,
              fit: BoxFit.cover,
            ),
            titleSection,
            textSection,
          ],
        ),
      ),
    );
  }
}
```

그동안 만든 위젯들을 앱 바의 밑에 리스트뷰로 그려줄 수 있습니다.

우선 이미지를 pubspec.yaml에 작성했던 이미지 주소를 가져와 에셋을 등록합니다.

마지막으로 그동안 만든 위젯 타입의 변수들을 보이고 싶은 차례대로 children에 나열합니다.