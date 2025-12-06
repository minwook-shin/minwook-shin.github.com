---
layout: post
title: flutter로 간단한 상호 작용 앱 만들어보기
---

오늘은 어제 만든 간단한 레이아웃 앱을 이용하여 버튼을 눌러서 상호작용하는 앱으로 바꾸려고 합니다.

이 포스팅을 따라 오신다면 여러분도 데이터를 가진 안드로이드, ios 앱을 동시에 만들어 볼 수 있습니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱을 구동할 장치를 준비해야 합니다.

준비했으면, 미리 기기를 ide에 연결해줍니다.

## 이미지 파일 준비하기

어제와 동일하게 앱에 사용될 이미지를 준비해줍니다.

이미지는 어제 포스팅에서 내려받을 수 있습니다.

## pubspec.yaml 작성하기

```yaml
flutter:
  uses-material-design: true
  assets:
    - lake.jpg
```

어제처럼 이미지를 에셋에 등록하기 위해 위와 같이 등록시켜줍니다.

## main.dart 작성하기

이제 메인 코드를 작성해보아야 합니다.

```dart
import 'package:flutter/material.dart';
```

flutter/material 패키지를 사용하겠다는 것이지만, 대부분의 앱에서 반드시 가져와야 하는 필수 패키지입니다.

```dart
void main() => runApp(MyApp());
```

앱이 실행되면 main함수가 수행되면서 MyApp 객체를 실행하게 되는 과정까지 어제와 같습니다.

```dart
class FavoriteWidget extends StatefulWidget {
  @override
  _FavoriteWidgetState createState() => _FavoriteWidgetState();
}
```

이제 StatefulWidget이라는 것이 등장하는데, 어제 작성할 때에 가져온 StatelessWidget와는 다릅니다.

버튼, 슬라이더, 체크 박스와 같이 사용자와 상호작용하여 위젯이 변화한다면 stateful이라고 하며, 기존 고정되있는 디자인은 Stateless이라고 합니다.

createState 메소드를 오버라이딩하여 _FavoriteWidgetState 객체를 생성해줍니다.

언더바를 사용하면 해당 클래스는 private 합니다.



```dart
class _FavoriteWidgetState extends State<FavoriteWidget> {
  bool _isFavorited = true;
  int _favoriteCount = 1;

  void _toggleFavorite() {
    setState(() {
      if (_isFavorited) {
        _favoriteCount -= 1;
        _isFavorited = false;
      } else {
        _favoriteCount += 1;
        _isFavorited = true;
      }
    });
  }
```

State 객체에서는 변수 값을 저장하는 것처럼 위젯의 상태는 State 객체에 저장되어 관리되므로 클래스도 기존의 위젯과 따로 만들어줍니다.

사용자의 특정 행동때문에 위젯이 변경되면 setState 메소드를 불러옵니다.

```dart
  @override
  Widget build(BuildContext context) {
    return Row(
      children: [
        Container(
          child: IconButton(
            icon: (_isFavorited ? Icon(Icons.star) : Icon(Icons.star_border)),
            color: Colors.red,
            onPressed: _toggleFavorite,
          ),
        ),
        SizedBox(
          width: 18.0,
          child: Container(
            child: Text('$_favoriteCount'),
          ),
        ),
      ],
    );
  }
}
```

build 메소드를 오버라이딩하여 아이콘과 숫자를 나타내는 텍스트를 그려주게 됩니다.

IconButton을 사용하여 onPressed 속성을 부여해줍니다.

이로서 아이콘이 눌렸을 때에 _toggleFavorite 메소드가 호출되며, 이 메소드의 setState 메소드가 최종적으로 호출되어 수행됩니다.

텍스트는 이전과 다르게 SizedBox로 묶은 이유는 숫자간의 서로 다른 간격을 고려하여 미리 간격을 벌려두기 위함입니다.

```dart
FavoriteWidget()
```

이제 Stateful인 위젯인 StatefulWidget 타입인 FavoriteWidget 메소드를 어제 작성했던 아이콘과 텍스트 자리에 배치하면 됩니다.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
```

MyApp은 StatelessWidget을 상속받아서 앱의 화면에 정적 위젯을 그리는 담당을 합니다.

build 메소드를 오버라이딩하여 이 클래스에 포함된 위젯들을 반환합니다.

다른 코드는 어제와 동일하므로 titleSection 위젯만 따로 보겠습니다.

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
          FavoriteWidget(),
        ],
      ),
    );
```

FavoriteWidget 메소드를 어제 코드에서 아이콘을 위치한 곳에 넣어줍니다.

이제 앱을 핫 리로드하면 아이콘 토글이 가능해집니다.