---
layout: post
title: 안드로이드에서 코틀린 object로 Constants 불러오기
---

오늘은 안드로이드에서 코틀린 object로 Constants를 불러와서 사용하는 방법을 알아보려 합니다.

## 개요

코틀린은 static라는 단어가 존재하지 않기 때문에, 싱글톤을 정의하거나 companion object를 사용할 수 있지만, 
오늘은 object를 사용한 싱글톤으로 Constants 클래스를 만들어 상수 모음집을 만들어보려 합니다.

## object

object를 사용하여 클래스를 정의할 때에 객체를 만들 수 있습니다.

```kotlin
object test {
    fun show() {
        val arr = arrayListOf<String>()
        arr.add("test")
        println(arr.toString())
    }
}
```

위와 같이 만들고

```java
test.show()
```

위와 같이 메소드에 직접 접근할 수 있으며, 변수도 마찬가지로 접근할 수 있습니다.

## Constants 파일 만들기

object 클래스로 상수를 저장해보려 합니다.

예시로 REQUEST_CODE 라는 상수가 필요하다고 가정을 하면, object 클래스에 const val의 상수를 선언하시면 됩니다.

```kotlin
object Constants{
    const val REQUEST_CODE = 1
}
```

Constants에 온점(.)을 찍으면 위 object 클래스에서 만들었던 상수가 보이게 됩니다.

```java
Constants.REQUEST_CODE // 1
```

코드에서 이렇게 작성하면 됩니다.