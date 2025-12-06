---
layout: post
title: 코틀린으로 작성한 안드로이드 데이터 바인딩에 대하여 알아보기
---

오늘은 안드로이드에서의 데이터바인딩을 코틀린으로 사용하는 방법을 알아보려 합니다.

## dependency 추가

```java
apply plugin: 'kotlin-kapt'
```

```java
android {
    dataBinding {
        enabled true
    }
}
```

```java
dependencies {
kapt 'com.android.databinding:compiler:3.1.3'
}
```

kotlin-kapt 플러그인을 추가해주고, dataBinding을 활성화해줍니다.

그리고 dependencies에 "kapt 'com.android.databinding:compiler:3.1.3'"를 추가해줍니다.

위 예시에 나온 데이터 바인딩 컴파일러 버전 3.1.3는 자신의 gradle 버전과 맞추어야 한다고 합니다.


## layout 설정

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">

    <data></data>

    <android.support.constraint.ConstraintLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".view.MainActivity">
    </android.support.constraint.ConstraintLayout>
</layout>
```

기존의 레아이웃과 별 다를 바가 없긴 하지만, 하나 다른 것은 ```<layout>``` 태그로 감싼다는 것입니다.

layout으로 기존의 코드를 감싸면 데이터바인딩을 사용할 수 있는 레이아웃이 됩니다.

그리고 data 태그를 통해 클래스를 가져와서 값을 ui에 출력할 수도 있습니다.

## activity 설정

```kotlin
class MainActivity : BaseActivity() {
    private val bind by lazy { DataBindingUtil
            .setContentView<ActivityMainBinding>(this@MainActivity, R.layout.activity_main) }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        bind

        bind.logInButton.setOnClickListener {}
    }
}
```

일반적으로는 ```val bind = DataBindingUtil.setContentView<ActivityMainBinding>(this, R.layout.activity_main)``` 정도만 해주어도 되지만, 
위와 같이 lazy를 사용하게 되면 onCreate 메소드에는 코드를 매번 할당할 필요없이 해당 클래스 전역에서 bind를 사용할 수 있게됩니다.

bind에서 해당 레이아웃 id를 불러들일 수 있습니다.