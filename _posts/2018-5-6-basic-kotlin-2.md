---
layout: post
title: Kotlin의 기본 문법 알아보기 2
---

오늘은 어제에 이어서 코틀린의 제어문부터 대하여 알아보려고 합니다.

## if 

if문은 자바와 같습니다.

```java
fun main(args: Array<String>) {
    var i : Int = 1;
    if(i == 1){
        println("1")
    }
    else if(i == 2){
        println("2")
    }
    else{
        println("other")
    }
}
```

## when

타 언어의 swith문과 유사합니다.

```java
fun main(args: Array<String>) {
    var i : Int = 1;
    when(i){
        1 -> println("hello")
        2 -> println("world")
        else -> {println("other")}
    }
}
```

## for

for문에는 range for와 collection for 문이 있습니다.

range for문은 범위 연잔자로 범위를 정하고 일정한 간격을 띄어서 반복합니다.

```java
fun main(args: Array<String>) {
    for(i in 1..10 step 3){
        println(i)
    }
}
```

collection for 문은 배열을 차례대로 반환합니다.

타 언어의 for each와 유사합니다.

```java
fun main(args: Array<String>) {
    var c : Array<Int> =  arrayOf(1,2,3)
    
    for(i in c){
        println(i)
    }
}
```

## while

while문은 자바는 물론이고 타언어와 유사합니다.

```java
fun main(args: Array<String>) {
    var i : Int = 0
    while(true){
        println("hello")
        i++
        if(i == 5){
            break
        }
    }
}
```

## 함수

함수는 타 언어와 비슷하지만, 코틀린의 특성상 함수명 뒤에 함수의 반환 타입이 옵니다.

```java
fun main(args: Array<String>) {
    test(5)
}

fun test(i :Int) : Int {
    println(i)
    return 0
}
```

## NULL SAFETY

코틀린에는 nullpoint exception을 방지하는 대책이 있습니다.

변수에 직접 접근을 차단하여, 일부의 경우에는 느낌표로 강제 해제해야합니다.

만약 null을 만난다면 null을 표시해줍니다.

```java
fun main(args: Array<String>) {
    var i : Int? = 5;
    var i2 : String? = "hello"
    var i3 : String? = null
    
    println(i)
    println(i2)
    println(i3)
    println(i2!!.length)
}
```