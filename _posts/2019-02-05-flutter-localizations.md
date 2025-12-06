---
layout: post
title: flutter 현지화(l10n) 적용해보기
---

오늘은 flutter에서 앱을 만들 때에 현지화로 다국어를 지원하는 방법을 알아보려 합니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱으로 테스트할 장치를 준비해야 합니다.

준비했으면, 미리 기기 또는 에뮬레이터를 ide에 연결해줍니다.

## pubspec.yaml 작성하기

```yaml
dependencies:
  flutter:
    sdk: flutter
  flutter_localizations:
    sdk: flutter
```

앱의 현지화를 위해 flutter_localizations 패키지를 추가해줍니다.

## main.dart 앱 코드 작성하기

메인 코드를 작성해야 합니다.

```dart
import 'package:flutter/material.dart';
import 'package:flutter/foundation.dart' show SynchronousFuture;
import 'package:flutter_localizations/flutter_localizations.dart';
```

로우 레벨의 유틸리티와 함수들을 모아둔 foundation 패키지에서 SynchronousFuture를 사용하고, flutter_localizations 패키지를 가져옵니다.

```dart
class LocalizationsClass {
  final Locale locale;
  LocalizationsClass(this.locale);
```

LocalizationsClass 클래스와 Locale 변수를 생성해줍니다.

```dart
  static Map _local = {
    'en': {'title': 'Hello'},
    'ko': {'title': '안녕'}
  };
```

private한 Map을 리터럴 형식으로 미리 지정하여 만들어줍니다.

키는 언어 코드로 찾게 할 수 있으며, 값은 문자열과 문자열 쌍으로 지정합니다.

```dart
  String get title {
    return _local[locale.languageCode]['title'];
  }
}
```

이로서 해당 앱에서 언어 코드가 인식되어 getter를 수행하면 map의 키로 값을 찾아서 번역된 문자열이 반환되게 됩니다.

```dart
class LocalizationsDelegateClass
    extends LocalizationsDelegate<LocalizationsClass> {
```

LocalizationsDelegate 클래스를 상속받아서 isSupported, load, shouldReload 메소드를 오버라이딩해주어야 합니다.

```dart
  @override
  bool isSupported(Locale locale) => ['en', 'ko'].contains(locale.languageCode);
```

isSupported 메소드는 contains으로 언어 코드가 포함되었는지 확인해주는 역할을 합니다.

```dart
  @override
  Future<LocalizationsClass> load(Locale locale) {
    return SynchronousFuture<LocalizationsClass>(LocalizationsClass(locale));
  }
```

load 메소드는 언어를 불러오는 역할을 합니다.

```dart
  @override
  bool shouldReload(LocalizationsDelegateClass old) => false;
}
```

shouldReload 메소드는 Delegate의 자원을 다시 불러와야 하는지에 대하여 bool 형식으로 지정하는 역할을 합니다.

만약 true로 설정한다면, 현지화한 위젯에 종속된 위젯들을 다시 불러오게 됩니다.

```dart
void main() {
  runApp(MaterialApp(
      localizationsDelegates: [
        LocalizationsDelegateClass(),
        GlobalMaterialLocalizations.delegate,
        GlobalWidgetsLocalizations.delegate
      ],
```

현지화를 할 때에 지정할 Delegates를 넣어줍니다.

```dart
      supportedLocales: [Locale('en', 'US'), Locale('ko', '')],
```

현지화할 언어들의 목록을 지정하여 넣어줍니다.

기본 값은 en-US이며, ko로 한국어를 설정할 수 있습니다.

```dart
      home: MyAppPage()));
}
```

MyAppPage 객체를 MaterialApp으로 감싸서 시작합니다.

```dart
class MyAppPage extends StatefulWidget {
  @override
  _MyHomePageState createState() => _MyHomePageState();
}
```

StatefulWidget으로 버튼, 슬라이더, 체크 박스처럼 사용자와 상호작용하여 위젯이 변화하는 것을 묶어줍니다.

createState 메소드를 오버라이딩하여 _MyHomePageState 객체를 생성해줍니다.

```dart
class _MyHomePageState extends State<MyAppPage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
          title: Text(
              Localizations.of<LocalizationsClass>(context, LocalizationsClass)
                  .title)),
    );
  }
}
```

테스트를 위해 앱에서 AppBar만 표시해보았습니다.

Text 생성자로 현지화된 title getter를 수행합니다.

## iOS 앱에서 특정 언어로 바꾸고 싶을 때

runner 폴더에 존재하는 info.plist 파일에서 CFBundleDevelopmentRegion 키를 가진 값을 수정하면 됩니다.