---
layout: post
title: flutter 생체 인증 사용하기
---

오늘은 flutter로 지문 인식이나 페이스id를 사용할 수 있는 local_auth 패키지에 대하여 알아보려 합니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱으로 테스트할 장치를 준비해야 합니다.

준비했으면, 미리 기기 또는 에뮬레이터를 ide에 연결해줍니다.

## pubspec.yaml 작성하기

```yaml
dependencies:
  flutter:
    sdk: flutter
  local_auth:
```

flutter 앱에 지문인식이나 페이스id를 사용하기 위해 local_auth 패키지를 pubspec.yaml에 작성해줍니다.

## android

안드로이드는 AndroidManifest.xml 파일에 지문 인식에 대한 권한을 추가해주어야 합니다.

```xml
<uses-permission android:name="android.permission.USE_FINGERPRINT"/>
```

## ios

ios도 FaceID 권한 설명을 첨부해야 하므로,
Runner 폴더의 Info.plist 파일에 NSFaceIDUsageDescription를 작성해주어야 합니다.

```xml
<key>NSFaceIDUsageDescription</key>
<string>Why is my app authenticating using face id?</string>
```

## main.dart 앱 코드 작성하기

```dart
import 'package:flutter/material.dart';
import 'package:local_auth/local_auth.dart';
```

앱에 머티리얼 위젯을 추가할 수 있는 material 패키지와 생체 인증을 할 수 있게 해주는 local_auth 패키지를 가져옵니다.

```dart
void main() => runApp(MaterialApp(home: MyApp()));
```

앱을 MaterialApp으로 감싼 MyApp으로 구동합니다.

```dart
class MyApp extends StatefulWidget {
  @override
  _MyAppState createState() => _MyAppState();
}
```

StatefulWidget으로 버튼, 슬라이더, 체크 박스처럼 사용자와 상호작용하여 위젯이 변화하는 것을 묶어줍니다.

createState 메소드를 오버라이딩하여 \_MyAppState 객체를 생성해줍니다.

```dart
class _MyAppState extends State<MyApp> {
  bool _isLocalAuth;
  String _authorized;
```

생체 인증이 되는 지에 대하여 bool 변수를 만들어주고, 문자열도 선언해줍니다.

```dart
  @override
  initState() {
    super.initState();
    checkBio();
  }
```

initState를 오버라이딩하여 앱이 구동했을 때에 checkBio라는 함수를 수행하게 합니다.

이로 인하여 해당 앱이 켜지면서 생체 인증을 할 수 있는 수단이 기기에 존재하는 지 확인할 수 있습니다.

```dart
  checkBio() async {
    var isLocalAuth;
    isLocalAuth = await LocalAuthentication().canCheckBiometrics;
    setState(() {
      _isLocalAuth = isLocalAuth;
    });
  }
```

checkBio를 비동기 함수로 만든 다음, LocalAuthentication의 canCheckBiometrics으로 기기에 생체 인증 수단이 있는지 확인합니다.

만약 지문 인식과 같은 방법이 존재한다면 true가 반환됩니다.

```dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Column(
        children: <Widget>[
          Text(_isLocalAuth.toString()),
          Text(_authorized == null ? "" : _authorized),
```

Text 생성자로 생체 인증이 가능한 지 확인하고, 인증이 된 상태인지도 확인해볼 수 있습니다.

두번째 Text 생성자에는 삼항 연산자로 \_authorized가 null이 아닐 때만 값을 출력합니다.

```dart
          OutlineButton(
              child: Text('Authenticate'),
              onPressed: () async {
                bool authenticated = false;
                authenticated = await LocalAuthentication()
                    .authenticateWithBiometrics(
                        localizedReason: '지문 인식을 진행해주십시오.');
                setState(() {
                  _authorized = authenticated ? 'Authorized' : 'Not Authorized';
                });
              })
        ],
      ),
    );
  }
}
```

버튼을 생성해주고, 해당 버튼이 눌리면 LocalAuthentication의 authenticateWithBiometrics으로 인하여 지문 인식 화면을 출력합니다.

지문 인식에 성공하면 true로 반환되고, 인식에 실패하면 false가 반환됩니다.

해당 코드에서는 삼항 연산자가 수행되어 인증이 되었는지 확인할 때에 setState에 의하여 화면이 새로 그려지게 됩니다.
