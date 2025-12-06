---
layout: post
title: 코틀린으로 작성한 안드로이드 application 클래스 활용하기
---

오늘은 코틀린으로 안드로이드에서 application 클래스를 활용하여 각각의 엑티비티에서 공통되게 사용할 수 있게 설정해보고 사용해보려 합니다.

## 서론

어플리케이션안에서 공동으로 멤버 변수나 메소드를 사용할 수 있게 해주는 공유 클래스를 Application Class라고 불립니다. 

## 클래스 구현

```kotlin
class App : Application() {

    init {
        INSTANCE = this
    }

    override fun onCreate() {
        super.onCreate()
    }

    companion object {
        lateinit var INSTANCE: App
    }
}
```

생성자에서는 INSTANCE = this라고 하고, 
클래스 타입의 정적 변수를 lateinit로 초기화를 나중에 해줍니다.


## Manifest

```xml
<application
android:name=".App">
</application>
```

android:name을 구현한 클래스의 이름으로 등록합니다.

## 사용

```java 
App.INSTANCE.getString()
```

Context가 필요한 곳에 App.INSTANCE를 불러와서 사용할 수 있습니다.

```java
toast(App.INSTANCE.user.userid)
```

앱이 실행되었을 때에 서로 다른 엑티비티에서 같은 객체를 가져다가 사용할 수 있습니다.
