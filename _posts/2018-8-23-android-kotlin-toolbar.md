---
layout: post
title: 코틀린으로 안드로이드 Toolbar 만들어보기
---

오늘은 안드로이드 프로젝트를 처음 만든 순간에 있는 ActionBar를 지우고 Toolbar를 추가해보겠습니다.

## ActionBar 지우기

```xml
<resources>
    <style name="AppTheme" parent="Base.Theme.AppCompat.Light.DarkActionBar">
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
        <item name="windowActionBar">false</item>
        <item name="windowNoTitle">true</item>
    </style>
</resources>
```

windowActionBar 속성과 windowNoTitle 속성을 각각 false, true로 맞추어줍니다.

스타일의 parent가 기존 프로젝트와 다른 이유는 예제코드가 androidx 패키지로 바꾼 프로젝트라 만약 조금 다르더라도 무시하시면 됩니다.

## Toolbar 추가

```xml
<Toolbar
        android:id="@+id/toolbar"
        android:layout_height="wrap_content"
        android:layout_width="match_parent"
        android:minHeight="?attr/actionBarSize"
        android:elevation="2dp">

    <TextView
        android:id="@+id/toolbarTextView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:longClickable="false"
        android:text="@string/app_name"/>

</Toolbar>
```

elevation로 고도를 설정하여 

## toolbar 앱 이름 삭제

만약 TextView로 따로 앱 이름을 넣지 않았는데도 앱 이름이 툴바에 나타나는 경우에는 아래처럼 false로 설정해줍니다.

```kotlin
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        setSupportActionBar(toolbar)
        supportActionBar?.setDisplayShowTitleEnabled(false)
    }
}
```

## 툴바 이벤트

툴바를 클릭할 때에 이벤트를 발생시키고 싶으면 

```java
android:clickable="true"
android:focusable="true"
```

해당 속성을 true로 추가해줍니다.

```kotlin
toolbarTextView.setOnClickListener {
    //method
}
```

엑티비티에서 해당 id로 클릭 이벤트 리스너를 등록하면 됩니다.