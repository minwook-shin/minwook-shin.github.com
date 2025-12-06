---
layout: post
title: 코틀린으로 작성한 확장 함수를 안드로이드에서 사용해보기
---

오늘은 코틀린에서 사용할 수 있는 확장 함수라는 것을 안드로이드에서 사용해보려 합니다.

## 개요

```kotlin
fun (확장하려는 대상 클래스).(추가하려는 메소드)(){
   // 내용
}
```

확장 함수는 확장하려는 클래스의 이름과 추가하려는 새로운 메소드를 적으면 굳이 클래스를 상속받지 않고도 마치 그 클래스의 메소드인 듯이 사용할 수 있습니다.

## 안드로이드 사용법

안드로이드에서도 예외가 아닙니다.

```kotlin
object ExtensionFunction{
    fun Activity.custom_toast(text : String){
        longToast("$text!!")
    }
}
```

Activity 클래스에서 확장하려고 위 코드와 같이 구현하면 모든 엑티비티에서는 엑티비티에서 제공하는 메소드처럼 자유롭게 쓸 수 있습니다.

당연히 String 클래스에도 확장함수를 적용할 수 있습니다.

```java
class MainActivity : BaseActivity() {
    private val bind by lazy { DataBindingUtil.setContentView<ActivityMainBinding>(this, R.layout.activity_main) }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        bind
        
        custom_toast("hello,world")
    }
}
```

위 예시는 AppCompatActivity 클래스를 상속받은 BaseActivity를 구현한 것을 이용한 것며, 데이터 바인드를 적용한 모습입니다.

중요한 것은 onCreate 메소드에서 기본적인 메소드처럼 작동한다는 부분입니다.
