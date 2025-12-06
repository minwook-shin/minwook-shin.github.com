---
layout: post
title: flutter로 만든 앱을 안드로이드 apk로 배포하기
---

오늘은 flutter 프로젝트를 안드로이드에서 설치가능한 apk로 만드는 방법을 작성하려 합니다.

이 방법으로 릴리즈하면 apk 파일과 키 파일로 구글 플레이에 앱을 제출할 수 있게 됩니다.

이번 포스트는 제가 안드로이드 앱을 출시하면서 까먹을까봐 작성한 목적이 있기 때문에 다소 설명이 부족할 수도 있습니다.

만약 특정 부분에서 오류가 난다면, 구글의 [공식 레퍼런스 문서](https://flutter.io/docs/deployment/android)를 참고하시기 바랍니다.

## 앱 디버그 해보기

먼저 앱이 구동이 잘 되는지 확인해봅니다.

이 과정에서는 에뮬레이터나 실기기나 상관없습니다.

## 버전 명시하기

해당 apk 파일의 버전을 정하기 위해 pubspec.yaml 파일에서 앱의 버전과 이름을 꼭 확인합니다.

이를 기반하여 앱의 업데이트가 진행되기 때문에 잘 작성해야 합니다.

## 권한 확인하기

안드로이드 폴더에 있는 AndroidManifest.xml 파일을 찾아서 앱에서 쓰는 권한만 추가되어 있는지 확인합니다.

flutter 기본 프로젝트에는 INTERNET 권한이 포함되어 있기 때문에, 자신의 앱이 인터넷을 사용하지 않는다면 android.permission.INTERNET 코드를 제거하시기 바랍니다.

## 이름 확인하기

안드로이드 폴더에 있는 AndroidManifest.xml 파일을 찾아서 android:label을 앱의 이름으로 잘 작성되어 있는지 확인합니다.

## 빌드 파일 고치기

기존에 디버그 용으로 작성된 빌드 파일도 약간의 수정을 거쳐야 합니다.

build.gradle 파일을 찾아서 applicationId를 고유한 앱 아이디로 바꾸어줍니다. 

이 앱 아이디는 한번 플레이 스토어에 올리면 다시 수정할 수 없으므로 신중하게 작성해야 합니다.

그 외에 sdk 버전도 잠깐 체크해줍니다.

## 아이콘 생성하기

안드로이드 리소스인 res의 mipmap 관련 폴더에 기본 리소스와 대체 리소스를 넣어서 앱의 아이콘을 준비해줍니다.

그리고 AndroidManifest.xml 파일의 android:icon에서 방금 저장한 아이콘의 이름을 지정해줍니다.

## 앱에 설정 확인하기

앱의 아이콘이나 버전을 확인하기 위해서 에뮬레이터로 잠시 확인해줍니다.

안드로이드 장치나 에뮬레이터를 ide에 연결해주고, 아래의 명령어로 디버그 앱을 설치하고 실행합니다.

```
flutter run
```

안드로이드 기기에서 앱의 정보를 보고, 잘 설정되었는지 확인합니다.

## 앱 사인하기

플레이 스토어에 업로드하려면 앱에 자신의 디지털 서명 키를 사인해야 합니다.

만약 이 과정을 처음 진행해서 keystore가 없다면, 아래의 명령어를 입력하여 생성해줍니다.

```
keytool -genkey -v -keystore ~/key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias key
```

[생성된 keystore는 모두에게 공개하면 안됩니다.]

keytool은 자바 기반으로 동작하므로, 윈도우와 같이 jdk 경로를 따로 설정해야 되는 경우에는 추가적인 작업이 필요할 수도 있습니다.

마지막으로 앱의 안드로이드 폴더에 key.properties 파일을 생성해줍니다.

```properties
storePassword=[서명을 만들 때에 작성한 비밀번호]
keyPassword=[서명을 만들 때에 작성한 비밀번호]
keyAlias=key
storeFile=[key store 파일 위치]
```

[보통 key store는 ```/Users/[사용자]/key.jks``` 경로에 저장됩니다.]

## 빌드 파일에 서명 관련 작성하기

아래 코드를 안드로이드 폴더의 앱 폴더에 존재하는 build.gradle 파일에서 android 윗 라인에 붙입니다.

이로 인해 key.properties 파일을 불러오게 됩니다.

```
def keystoreProperties = new Properties()
def keystorePropertiesFile = rootProject.file('key.properties')
if (keystorePropertiesFile.exists()) {
	keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
}
```

아래 코드를 build.gradle 파일에서 buildTypes 윗 라인에 붙입니다.

key.properties 파일의 필드를 불러와서 사용하게 됩니다.

```
signingConfigs {
	  release {
	      keyAlias keystoreProperties['keyAlias']
	      keyPassword keystoreProperties['keyPassword']
	      storeFile file(keystoreProperties['storeFile'])
	      storePassword keystoreProperties['storePassword']
	  }
}
```

그리고 buildTypes의 release에는 디버그 코드를 signingConfigs.release로 교체해줍니다.

## Proguard 활성화

앱의 난독화를 위해서 Proguard를 활성화해야 합니다.

안드로이드 앱 폴더에서 proguard-rules.pro 파일을 만들어주고 아래 코드를 넣어줍니다.

```pro
#Flutter Wrapper
-keep class io.flutter.app.** { *; }
-keep class io.flutter.plugin.**  { *; }
-keep class io.flutter.util.**  { *; }
-keep class io.flutter.view.**  { *; }
-keep class io.flutter.**  { *; }
-keep class io.flutter.plugins.**  { *; }
```

이렇게 되면 Flutter의 엔진 라이브러리만 보호되므로 나머지는 사용자 스스로 추가하시면 됩니다.

마지막으로 안드로이드 앱 폴더의 build.gradle 파일에서 Proguard를 사용한다고 선언합니다.

아래 코드를 buildTypes의 release에서 추가로 작성합니다.

```
minifyEnabled true
useProguard true

proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
```

## apk 파일 만들기

여기까지 모든 설정이 끝났다면, apk 파일로 빌드해야 합니다.

아래 명령어를 진행하여 apk 파일을 만들어줍니다.

```
flutter build apk
```

build/app/outputs/apk/release 폴더에 app-release.apk 파일이 존재합니다.

## 플레이 스토어 업로드하기

이제 위에서 만든 apk 릴리즈 파일과 keystore 파일을 가지고 구글 플레이 콘솔에 접속하여 앱을 출시하면 됩니다.

## 앱 업데이트하기

업데이트는 pubspec.yaml에서 앱의 버전 코드를 올리고, 다시 apk 빌드를 진행하면 됩니다.

만약 "1.0+1"이라고 출시했으면 "1.0+2" 혹은 "1.1+2"와 같이 앱의 버전 코드를 정수형으로 올려야 합니다.

apk 빌드가 진행되고, 다시 플레이 스토어 콘솔에서 배포를 진행합니다.