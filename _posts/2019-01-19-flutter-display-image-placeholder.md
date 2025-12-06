---
layout: post
title: flutter로 이미지 가져오면서 placeholder 설정하기
---

오늘은 flutter의 외부 패키지인 cached_network_image로 placeholder를 설정하고, 이미지를 가져와서 출력하는 앱을 만들어보려 합니다.

원래 cached_network_image 패키지는 이름처럼 이미지를 캐싱해주는 기능이 주를 이루고 있으나, placeholder를 위젯으로 사용하거나 페이드 인아웃 효과도 별도로 부여할 수 있기 때문에 이 패키지를 사용했습니다.

(간단하게 페이드 인 효과를 주고 싶다면 flutter에는 기본적으로 FadeInImage이라는 패키지가 존재하긴 합니다.)

이 포스팅을 따라 오신다면 여러분도 특정 이미지를 인터넷에서 가져와서 출력할 때 사용자 경험을 좋게 만들어놓은 앱을 안드로이드, ios 앱으로 동시에 만들어 볼 수 있습니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱을 구동할 장치를 준비해야 합니다.

준비했으면, 미리 기기를 ide에 연결해줍니다.

## pubspec.yaml 작성하기

외부 패키지인 cached_network_image를 사용하기 위해서는 pubspec 파일에서 의존성을 추가해야 합니다.

```yaml
dependencies:
  flutter:
    sdk: flutter
  cached_network_image:
```

pubspec 파일을 작성한 뒤에는 터미널에서 아래와 같이 적용해주어야 합니다.

```bash
$ flutter packages get
```

## main.dart 작성하기

이제 메인 코드를 작성해보아야 합니다.

```dart
import 'package:flutter/material.dart';
```

flutter/material 패키지를 사용하겠다는 것이지만, 대부분의 앱에서 반드시 가져와야 하는 필수 패키지입니다.

```dart
import 'package:cached_network_image/cached_network_image.dart';
```

pubspec의 dependencies에 정의한 cached_network_image를 가져옵니다.

```dart
void main() {
  runApp(MaterialApp(
    title: 'Image loading', 
    home: Scaffold(body: MyApp())));
}
```

처음 앱이 실행될 때 앱의 타이틀과 메인스크린 위젯을 출력합니다.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      alignment: Alignment.center,
```

Container 위젯으로 정렬을 정의해줍니다.

```dart
      child: CachedNetworkImage(
        placeholder: CircularProgressIndicator(),
```

Container 위젯의 하위에는 CachedNetworkImage 위젯으로 이미지를 기기에 캐싱할 수 있게 합니다.

이 위젯으로 인하여 기존에 미리 구현되어 있는 CircularProgressIndicator 위젯을 placeholder로 사용할 수 있습니다.

```dart
        imageUrl:
            'https://github.com/flutter/plugins/raw/master/packages/video_player/doc/demo_ipod.gif?raw=true',
```

imageUrl 속성으로 가져올 이미지 주소를 삽입합니다.

앞 포스팅에서 언급한 것처럼 gif 파일도 가능합니다.

```dart
        errorWidget: Icon(Icons.error),
```

오류가 났을 때를 대비한 아이콘 위젯 출력도 가능합니다.

```dart
        fadeInCurve: Curves.easeIn,
        fadeInDuration: Duration(seconds: 3),
      ),
    );
  }
}
```

flutter에 기본으로 들어있는 FadeInImage 패키지처럼 FadeIn 효과를 줄 수 있고, 추가로 효과를 주는 주기도 설정할 수 있습니다.

위 코드대로 작성하면 3초간 FadeIn 됩니다.

코드를 빌드한 뒤에 연속으로 앱을 구동하면 이미지를 불러오는 시간이 단축됨을 확인할 수 있습니다.