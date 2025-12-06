---
layout: post
title: 코틀린으로 작성한 안드로이드에서 Shared Preferences 사용하기
---

오늘은 안드로이드에서 Shared Preferences를 사용하는 방법을 코틀린으로 알아보려 합니다.

Application 클래스로 Shared Preferences를 전역에서 사용해보려 합니다.

## 개요

SharedPreferences는 간단한 문자열을 저장하기 좋은 저장 방식입니다.

어플리케이션이 삭제되기 전까지는 보존되기 때문에 최초 1회 실행을 검사하거나, 로그인 값을 저장하기에 유용합니다.

데이터의 구별은 파일 이름, 키값으로 구분됩니다.

## 클래스

```kotlin
private const val FILENAME = "prefs"
private const val PREF_TEXT = "Text"
class MySharedPreferences(context: Context) {
    private val prefs: SharedPreferences = context.getSharedPreferences(FILENAME, 0)
    var text: String
        get() = prefs.getString(PREF_TEXT, "")
        set(value) = prefs.edit().putString(PREF_TEXT, value).apply()
}
```

하나의 파일을 생성하는 getSharedPreferences를 SharedPreferences 타입의 객체를 만듭니다.

그리고 코틀린의 특성상 자동으로 getter와 setter를 만들어주기 때문에 이것들을 커스텀으로 바꾸어 줍니다.

getter면 getString으로 데이터를 가져오고, setter면 putString으로 데이터를 해당 상수에 맞는 파일에 기입합니다.

## Application

```kotlin
class App : Application() {
    init {
        INSTANCE = this
    }

    override fun onCreate() {
        prefs = MySP(applicationContext)
        super.onCreate()
    }

    companion object {
        lateinit var INSTANCE: App
        lateinit var prefs: MySharedPreferences
    }
}
```

앱 전역에서 사용하려면 Application 클래스를 상속받는 클래스를 만들어야 합니다.

방금 만든 MySharedPreferences 클래스로 객페를 만들어주고 초기화는 lateinit으로 미룹니다.

## 전역 사용법

```java
App.prefs.text = "hello world!"
```

처음에 만든 클래스에서 getter가 작동되어 PREF_TEXT 상수에 들어있는 이름의 파일에서 가져오게 됩니다.

```java
print(App.prefs.text.toSting())
```

처음에 만든 클래스에서 setter가 작동되어 PREF_TEXT 상수에 들어있는 이름으로 저장됩니다.


## 일반적인 사용법

```kotlin
val pref = this.getPreferences(0)
val editor = pref.edit()
```

물론 일반적인 방식으로 데이터들을 저장하고 가져올 수도 있습니다.

해당 엑티비티에서만 쓰이는 파일을 만들 수 있습니다. 

```java
editor.putString(Constants, "HELLO WORLD").apply()
```

위 코드는 예시로 hello world라는 문자열을 저장해주는 코드입니다.

```java
pintln(pref.getString(Constants, ""))
```

문자열을 가져와서 출력해줍니다.