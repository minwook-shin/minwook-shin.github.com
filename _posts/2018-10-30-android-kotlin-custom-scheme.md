---
layout: post
title: 코틀린 프로젝트로 안드로이드에서 Custom Scheme 사용해보기
---

오늘은 코틀린 안드로이드 프로젝트에서 만든 앱으로 접속하기 위한 Custom scheme를 구성해서 사용해보려 합니다.

## intro

http와 ftp처럼 요구되는 명세를 scheme라고 하며, 이를 이용하여 나만의 scheme를 만들 수 있습니다.

안드로이드에서 자신이 만든 Custom scheme를 이용하여 자신의 앱을 불러올 수 있게 만들 수 있습니다.

## manifast

```xml
<activity android:name=".view.MainActivity">
        <intent-filter>
            <action android:name="android.intent.action.MAIN" />
            <category android:name="android.intent.category.LAUNCHER" />
            <action android:name="android.intent.action.VIEW" />

            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:host="host" android:scheme="custom" />
        </intent-filter>
</activity>
```

예를 들어 메인 엑티비티로 불러오고 싶다면 manifast의 해당 엑티비티 태그에 카테고리를 추가하고, scheme와 host를 추가해줍니다.

이와 같이 작성하면 custom://host로 접속할 수 있습니다.

## activity

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_xml)
    intent?.let {
        val uri = intent.data
        if (uri != null) {
            val param: String? = uri.getQueryParameter("p1")
            longToast(param.toString())
        }
    }
}
```

링크를 클릭했을 시에 접속하는 엑티비티에 위와 같이 코드를 작성해줍니다.

만약 링크에 접속하면서 값을 가져오고 싶다면, custom://host?p1="value" 와 같이 링크를 구성하여 value를 가져올 수 있습니다.

## link

```html
<a href="custom://host">
```

앱이 설치된 안드로이드 기기의 브라우저에서 링크를 바로 작성하면 Custom scheme가 작동되지 않습니다.

html 태그로 하이퍼 링크를 만들어주면 정상적으로 작동하는 것을 확인할 수 있습니다.