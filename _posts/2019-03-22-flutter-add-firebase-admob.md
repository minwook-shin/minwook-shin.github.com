---
layout: post
title: flutter admob 광고 추가하기
---

오늘은 flutter로 만든 앱에 구글의 Firebase AdMob API를 사용하여 admob 광고를 첨부할 수 있는 firebase_admob 패키지에 대하여 알아보려 합니다.

이 포스팅을 쓰고 있는 현재는 배너 광고와 전면 광고, 그리고 보상형 광고만 지원합니다.

네이티브 광고는 아직 지원하지 않습니다.

## AndroidManifest.xml

샘플 admob 앱 아이디는 아래와 같으므로, 해당 아이디 값을 참고하여 AndroidManifest 파일에 메타 데이터를 기록해줍니다.

```
ca-app-pub-3940256099942544~3347511713
```

```xml
<meta-data
            android:name="com.google.android.gms.ads.APPLICATION_ID"
            android:value="ca-app-pub-3940256099942544~3347511713"/>
```

앱의 메인 코드에서 작성하는 FirebaseAdMob.instance.initialize에서도 같은 앱 아이디를 사용해야 합니다.

해당 포스트에서는 FirebaseAdMob.testAppId라는 상수를 이용하지만, 내부는 같은 아이디로 이루어져 있습니다.

```dart
  static final String testAppId = Platform.isAndroid
      ? 'ca-app-pub-3940256099942544~3347511713'
      : 'ca-app-pub-3940256099942544~1458002511';
```

testAppId의 내부를 보면 안드로이드의 경우와 아닐 경우로 나누어서 처리함을 알 수 있습니다.

## 보상형 광고의 차이점

배너 광고와 다르게 보상형 광고는 싱글톤으로 이루어져 있기 때문에 별도의 dispose 메소드가 필요없습니다.

## 에뮬레이터 및 기기 준비하기

안드로이드나 ios 앱으로 테스트할 장치를 준비해야 합니다.

준비했으면, 미리 기기 또는 에뮬레이터를 ide에 연결해줍니다.

## pubspec.yaml 작성하기

```yaml
dependencies:
  flutter:
    sdk: flutter
  firebase_admob:
```

flutter 앱에 광고를 추가하기 위하여 firebase_admob 패키지를 pubspec.yaml에 작성해줍니다.

## main.dart 앱 코드 작성하기

```dart
import 'package:flutter/material.dart';
import 'package:firebase_admob/firebase_admob.dart';
```

앱에 머티리얼 위젯을 추가할 수 있는 material 패키지와 광고를 추가해줄 수 있는 firebase_admob 패키지를 가져옵니다.

```dart
void main() {
  runApp(MyApp());
  FirebaseAdMob.instance.initialize(appId: FirebaseAdMob.testAppId);
}
```

MyApp 객체를 앱으로 구동합니다.

그리고 FirebaseAdMob의 인스턴스를 초기화해줍니다.

이 때 appId는 AndroidManifest에 작성한 아이디로 넣어야 합니다.

테스트 기기에서는 미리 정의된 testAppId를 이용합니다.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter admob',
      home: MyHomePage(),
    );
  }
}
```

머티리얼 디자인으로 감싸진 MyHomePage 객체를 화면에 그려줍니다.

```dart
class MyHomePage extends StatefulWidget {
  @override
  _MyHomePageState createState() => _MyHomePageState();
}
```

StatefulWidget으로 버튼, 슬라이더, 체크 박스처럼 사용자와 상호작용하여 위젯이 변화하는 것을 묶어줍니다.

createState 메소드를 오버라이딩하여 \_MyHomePageState 객체를 생성해줍니다.

```dart
MobileAdTargetingInfo targetingInfo = MobileAdTargetingInfo(
  keywords: <String>['flutterio', 'beautiful apps'],
  contentUrl: 'https://flutter.dev',
  childDirected: false,
  testDevices: <String>[],
);
```

네이티브 AdMob API의 타겟팅에 대한 정보를 static하게 만들어 줍니다.

키워드, 주소등 속성들이 포함됩니다.

```dart
class _MyHomePageState extends State<MyHomePage> {
  BannerAd banner = BannerAd(
    adUnitId: BannerAd.testAdUnitId,
    size: AdSize.smartBanner,
    targetingInfo: targetingInfo,
    listener: (MobileAdEvent event) {
      print("$event");
    },
  );
```

BannerAd로 배너 광고를 정의할 수 있습니다.

광고 유닛 아이디, 배너의 크기, 타겟팅, 리스너 콜백등을 설정할 수 있습니다.

```dart
  @override
  void initState() {
    super.initState();
    banner
      ..load()
      ..show(
        anchorType: AnchorType.bottom,
      );
```

미리 정의해둔 배너 광고를 로드하고, show로 앱에 출력시킬 수 있습니다.

```dart
    RewardedVideoAd.instance.load(
        adUnitId: RewardedVideoAd.testAdUnitId, targetingInfo: targetingInfo);
```

보상형 광고는 싱글톤으로 이루어져 있어서 바로 인스턴스를 불러와서 유닛 아이디와 타겟을 설정할 수 있습니다.

```dart
    RewardedVideoAd.instance.listener =
        (RewardedVideoAdEvent event, {String rewardType, int rewardAmount}) {
      if (event == RewardedVideoAdEvent.rewarded) {
        setState(() {
          print(rewardAmount);
        });
      }
    };
  }
```

리스너도 RewardedVideoAd의 인스턴스를 바로 불러와서 광고를 시청한 뒤에 rewarded되면 보상을 줄 수 있게 행동을 정의할 수 있습니다.

```dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
        body: OutlineButton(
      onPressed: () {
        RewardedVideoAd.instance.show();
      },
      child: Text("보상형 광고"),
    ));
  }
}
```

배너 광고는 show()를 한 뒤에 출력되기 때문에 별도의 위젯이 필요없고, 보상형 광고도 별도의 위젯이 필요 없지만 보여줘야 할 시기에 show()해주어야 합니다.
