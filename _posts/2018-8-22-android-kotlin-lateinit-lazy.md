---
layout: post
title: 안드로이드에서 코틀린 lateinit과 lazy 사용해보기
---

오늘은 코틀린에서 초기화를 지연시킬 때에 사용되는 lateinit과 lazy 키워드를 알아보려 합니다.

## 지연 초기화 장점

클래스 생성하고 변수가 초기화되는 것보다 지연 초기화를 통해 메모리의 이득을 볼 수 있고, 초기화 시에 null로 할 필요가 없을 때에 할 수 있습니다.

## lateinit

초기화를 늦게할 수 있는 프로퍼티입니다.

변경가능한 var에만 사용가능하며, null로 초기화를 할 수 없습니다.

```kotlin
class App : Application() {
    init {
        INSTANCE = this
    }
    companion object {
        lateinit var INSTANCE: App
    }
}
```

위 코드는 Application 클래스를 상속받은 App 클래스를 작성한 것으로서 companion object 키워드로 싱글톤인 변수를 lateinit로 초기화 지연했습니다.

그리고 App 클래스가 사용되는 순간에 생성자가 호출되어 초기화가 진행되게 됩니다.


## lazy

초기화를 늦게하는 Delegated 프로퍼티입니다.

Delegated 프로퍼티란, 프로퍼티에 대한 getter와 setter를 위임하여 위임받은 객체로 명령을 수행하는 것을 말합니다.

호출 시점의 by lazy에서 미리 정의한 내용에 의해서 초기화가 되며, 변경 불가능한 val에서만 사용이 가능하다는 특징이 있습니다.

by lazy는 Lazy<T>를 반환받아서 최초 1회만 lazy() 람다식을 실행하고, 이후에는 계속 이 식을 실행합니다.

```kotlin
private val bind: ActivityMainBinding by lazy {DataBindingUtil.setContentView<ActivityMainBinding>(this@MainActivity, R.layout.activity_main)}
```

위 코드는 DataBindingUtil에서 레이아웃을 setContentView하는 과정에 바인딩을 생성하는 코드입니다.

클래스 전역에 생성해야 클래스 내의 모든 함수에서 사용가능하기 때문에 전역에서 by lazy로 초기화 지연을 했습니다.

onCreate 메소드에서 최초 1회만 bind를 호출하면 by lazy에 미리 정의한 내용으로 초기화하게 됩니다.