---
layout: post
title: Kotlin의 class에 대하여 알아보기 1
---

오늘은 코틀린에서의 class에 대하여 알아보려 합니다.

## class 선언 방식

```java
class test(){
}
```


## class 구성

속성와 함수로 이루어져 있습니다.

```java
class test(){
    var a : Int = 1
    var b : Int = 2

    fun sum():Int{
        return a + b
    }
}
```

## class 활용

```java
class test(){
    var name : String = "test"
    var a : Int = 1
    var b : Int = 2
    fun sum():Int{
        return a + b
    }
}

fun main(args: Array<String>) {
    var real = test()
    var result = real.sum();
    println(result)
}
```

먼저 test라는 클래스를 만들고, 각종 속성을 선언하고 값을 초기화하며 대입합니다.

그리고 그 클래스의 함수를 작성합니다.

마지막으로 메인에서 test 클래스의 객체를 생성하면서 온점으로 함수에 접근할 수 있습니다.

> 다음 포스팅에서 이어집니다.