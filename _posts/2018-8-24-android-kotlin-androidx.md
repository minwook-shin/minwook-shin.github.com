---
layout: post
title: 코틀린 프로젝트로 기존 android support 패키지를 androidx 패키지로 변환해보기
---

오늘은 기존 com.android.support.*로 사용되는 android support 패키지를 androidx 패키지로 변환해보려고 합니다.

구글에서 Android Jetpack을 발표할 때에 기존 android support 패키지와 architecture 패키지 등을 통합하여 관리하고자 androidx로 패키지를 통합하겠다고 발표했습니다.

사실 안드로이드 스튜디오 3.2부터는 자동으로 변환할 수 있는 옵션(Refactor to AndroidX)를 제공하겠다고 했지만, 저는 안정 버전인 안드로이드 스튜디오로 androidx 변환을 수동으로라도 해보고싶었습니다.

## gradle

우선 해당  AndroidX refactoring [문서](https://developer.android.com/topic/libraries/support-library/refactor)를 참고하여 아래 의존성 패키지를 refactoring 해봅시다.

```java
dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    implementation"org.jetbrains.kotlin:kotlin-stdlib-jdk7:$kotlin_version"
    implementation 'com.android.support:appcompat-v7:28.0.0-rc01'
    implementation 'com.android.support.constraint:constraint-layout:1.1.2'
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'com.android.support.test:runner:1.0.2'
    androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.2'
}
```

아래는 AndroidX refactoring 문서를 참고한 결과입니다.

```java
dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    implementation"org.jetbrains.kotlin:kotlin-stdlib-jdk7:$kotlin_version"
    implementation 'androidx.appcompat:appcompat:1.0.0-rc01'
    implementation 'androidx.constraintlayout:constraintlayout:1.1.2'
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'androidx.test:runner:1.1.0-alpha4'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.1.0-alpha4'
}
```

gradle sync는 정상적으로 진행되나 바로 컴파일이 안됩니다.

Unresolved reference 라면서 패키지를 찾을 수 없을 때 나오는 오류를 수정하기 위해 자동 생성된 MainActivity를 먼저 보자면

```java
import android.support.v7.app.AppCompatActivity
```

이 패키지가 없어서 오류가 발생되므로

```java
import androidx.appcompat.app.AppCompatActivity
```

androidx 패키지로 바꾸어주면 정상적으로 빌드가 수행됩니다.

## xml

아마 레이아웃에서도 아래와 같은 오류가 생길겁니다.

레이아웃의 클래스를 androidx 패키지 경로로 바꿔주지 않았기 때문입니다.

```xml
The following classes could not be found:
- android.support.constraint.ConstraintLayout
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</android.support.constraint.ConstraintLayout>
```

위와 같은 ```android.support.constraint.ConstraintLayout``` 를 ```androidx.constraintlayout.widget.ConstraintLayout```로 변경해주시면 됩니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

그리고 ActionBar 부분도 수정해주어야 레이아웃 프리뷰도 동작합니다.

```java
The following classes could not be found:
- android.support.v7.app.WindowDecorActionBar 
```

```style.xml```파일로 가서

```xml
<resources>
    <style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
    </style>
</resources>
```

parent 부분을 ```Base.Theme.AppCompat.Light.DarkActionBar```로 변경해주시면 됩니다.

```xml
<resources>
    <style name="AppTheme" parent="Base.Theme.AppCompat.Light.DarkActionBar">
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
    </style>

</resources>
```

gradle과 layout까지 변경하고나면 레이아웃 프리뷰이 정상적으로 보이며,  프로젝트도 androidx 패키지로 빌드됨을 확인할 수 있습니다.