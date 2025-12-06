---
layout: post
title: 코틀린 프로젝트로 안드로이드에서 AboutLibraries 사용해보기
---

오늘은 코틀린 안드로이드로 프로젝트에서 사용한 라이브러리들을 알아서 기록해주는 라이브러리로 사용해보려 합니다.

AboutLibraries라고 불리는 라이브러리를 사용하면 자신의 프로젝트에서 사용중인 오픈소스 라이브러리들을 쉽게 엑티비티로 표기할 수 있습니다.

## 의존성

```java
    implementation "com.mikepenz:aboutlibraries:6.2.0"
```

AboutLibraries를 gradle에 넣어줍니다.

그리고 appcompat, cardview, recyclerview를 이 라이브러리의 의존성으로 넣어주어야 합니다.

```java
    implementation 'androidx.appcompat:appcompat:1.0.0'
    implementation "androidx.cardview:cardview:1.0.0"
    implementation "androidx.recyclerview:recyclerview:1.0.0"
```

일단 androidx 패키지를 사용한 패키지라면 위와 같이 작성합니다.

## 클릭 이벤트

```kotlin
Button.setOnClickListener {}
```

클릭할 때에 엑티비티를 띄우려면 위와 같이 작성하고 블록안에 아래와 같이 작성합니다.

## 엑티비티에서 엑티비티 띄우기

```kotlin
LibsBuilder()
    .withAutoDetect(true)
    .withLicenseShown(true)
    .withActivityStyle(Libs.ActivityStyle.LIGHT_DARK_TOOLBAR)
    .withAboutIconShown(true)
    .withAboutAppName("app name")
    .start(this)
```

각 라이브러리들의 라이선스를 출력하면서 자신의 프로젝트 앱 이름과 앱 아이콘도 같이 나타내는 엑티비티를 만들어주는 빌더입니다.

## 프래그먼트에서 엑티비티 띄우기

```kotlin
LibsBuilder()
    .withAutoDetect(true)
    .withLicenseShown(true)
    .withActivityStyle(Libs.ActivityStyle.LIGHT_DARK_TOOLBAR)
    .withAboutIconShown(true)
    .withAboutAppName("app name")
    .start(this.activity)
```

엑티비티에서 하는 작업과 다른 것은 프래그먼트의 엑티비티의 context를 얻어와 실행한다는 것입니다.

자바에서는 this.getActivity()입니다.

## 프래그먼트로 띄우기

```kotlin
val fragment : LibsFragment = LibsBuilder()
        .fragment();
```

엑티비티로 띄우지 않고, 프래그먼트로 띄울 수도 있습니다.
