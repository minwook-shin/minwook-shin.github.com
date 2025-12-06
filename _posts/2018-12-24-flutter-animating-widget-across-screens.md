---
layout: post
title: flutter로 위젯 애니메이션 앱 만들어보기
---

오늘은 flutter로 같은 위젯을 애니메이션하는 앱을 만들어보려합니다.

이 포스팅을 따라 오신다면 여러분도 화면 이동할 때의 위젯 애니메이션을 가진 안드로이드, ios 앱을 동시에 만들어 볼 수 있습니다.

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
void main() => runApp(App());

class App extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Transition',
      home: MainScreen(),
    );
  }
}
```

처음 앱이 실행될 때 앱의 타이틀과 메인스크린 위젯을 출력합니다.

```dart
class MainScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: GestureDetector(
        child: Hero(
          tag: '같은태그',
          child: Image.asset(
            'lake.jpg',
            fit: BoxFit.cover,
          ),
        ),
```

이미지 에셋으로 받아온 이미지를 Hero로 감싸서 태그를 지정합니다.

이 태그로 인해 같은 태그끼리 화면이 넘어갈 때 애니메이션을 형성할 수 있습니다.

만약 다음 페이지에 존재하는 Hero에서 태그가 다르다면 일반적인 화면 전환처럼 이루어집니다.

```dart
        onTap: () {
          Navigator.push(
            context,
            MaterialPageRoute(builder: (context) => DetailScreen()),
          );
        },
      ),
    );
  }
}
```

이미지를 감싼 Hero를 GestureDetector로 또 감싸서 onTap 속성을 사용할 수 있게 합니다.

GestureDetector를 이용했기 때문에, The named parameter 'onTap' isn't defined.라는며 인자가 없다는 오류가 출력되지 않습니다.

이미지를 누르면 DetailScreen로 넘어갑니다.

```dart
class DetailScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      body: Center(
        child: Hero(
          tag: '같은태그',
          child: Image.asset(
            'lake.jpg',
            fit: BoxFit.cover,
          ),
        ),
      ),
    );
  }
}
```

이미지를 누르면 DetailScreen 위젯이 출력되면서 뒤로가기가 들어간 앱 바와 이미지가 화면에 그려지게 됩니다.

이 이미지는 GestureDetector가 없기 때문에 이미지를 눌러도 반응이 없습니다.

이제 앱을 빌드하고 이미지를 눌러보면 위젯 애니메이션이 작동하는 것을 볼 수 있습니다.