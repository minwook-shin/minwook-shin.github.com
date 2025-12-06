---
layout: post
title: Kotlin의 class에 대하여 알아보기 4
---

오늘은 코틀린에서 데이터 클래스와 NESTED 클래스, NESTED 데이터 클래스에 대하여 알아보겠습니다.

## 데이터 클래스

데이터들만 가지는 클래스입니다.

자바에서는 생성자, getter/setter, equals, tostring 들을 직접 만들었지만, 코틀린에서는 데이터 클래스를 도입하여 자동으로 위와 같은 것들이 만들어집니다.

중괄호대신 소괄호가 사용됩니다.

```java
data class test(
    var number : Int,
    var name : String,
    var code : String
)

fun main(args: Array<String>) {
    var real = test(1,"name","kotlin")
    println(real.code)
    
    var real2 = real.copy()
    println(real2.toString())
    real2.name = "name2"
    println(real2.toString())
    println(real2.name.equals(real.name))
}
```

위 예시는 테스트라는 데이터 클래스를 만들고, 이 데이터 클래스에서 객체를 생성하여 getter/setter, 복사, 출력, 비교를 하는 코드입니다.

## NESTED 클래스

중첨 클래스라는 뜻으로, 클래스안에 클래스가 있다는 겁니다.

```java
class test{
    var name : String? = null
    var code : Code? = null
    class Code{
        var java : String? = null
        var kotlin : String? = null
    }
}

fun main(args: Array<String>) {
    var real = test()
    real.name = "namw"
    
    var code = test.Code()
    code.java = "system.out.plrintln"
    code.kotlin = "println"
    
    real.code = code
    
    println(real.code?.java)
    println(real.code?.kotlin)
}
```

이와 같이 중첩하여 구성할 수 있습니다.

## NESTED 데이터 클래스

데이터 클래스를 NESTED로 만들 수 있습니다.

이렇게 되면, 메인에서 간결하게 데이터를 묶어서 넣을 수 있습니다.

```java
data class test(
    var number : Int,
    var str : Str
){data class Str(
    var name : String,
    var code : String
	)
}

fun main(args: Array<String>) {
    var real = test(1,test.Str("kotlin","println"))
    println(real.toString())
}
```