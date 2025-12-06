---
layout: post
title: Kotlin의 기본 문법 알아보기 1
---

오늘은 코틀린 언어에 대한 기본적인 문법에 대하여 알아보도록 하겠습니다.

## 개요

JETBRAINS에서 2011년에 출시되어 개발했으며,  ANDROID STUDIO 3.0의 공식언어입니다.

쉽고 간결한, 자바와 호환가능하고 NULL POINT EXCEPTION에 대한 안정성이 강화되었다고 합니다.

## 문자

문자는 글자를 한 글자만 담으며, Char와 작은 따음표를 이용하여 표시합니다.

```java
fun main(args: Array<String>) {
    var c : Char = 'A'
    println(c)
}
```

## BOOL

true, false만 입력가능하며, Boolean 으로 선언합니다.

```java
fun main(args: Array<String>) {
    var A : Boolean = true
    println(A)
}
```

## 문자열

여러개의 문자를 담는 문자열이고, String과 큰 따음표를 이용하여 표시합니다.

```java
fun main(args: Array<String>) {
    var A : String = "hello"
    println(A)
}
```

## 배열 

2차원 데이터이며, Array<>로 생성합니다.

```java
fun main(args: Array<String>) {
    var A : Array<Int> = arrayOf(1,2,3)
    println(A[0])
}
```

## 상수

수정이 불가하며, val로 선언합니다.
초기 값이 반드시 필요합니다.

```java
fun main(args: Array<String>) {
   	val A : Int = 1;
    println(A)
}
```

## 연산자

대입 연산자와 산술 연산자, 그리고 비교 연산자는 타 언어와 같습니다.

범위 연산자는 비교하는 변수로서 in을 사용합니다.

```java
fun main(args: Array<String>) {
   var x = 5
   println(x in 1..10)
}
```

위 경우에는 x에 5가 대입되어 있으며, 1부터 10까지 변수 x가 있는지 boolean 값으로 나타냅니다.

> 지금까지 변수와 상수, 그리고 연산자에 대하여 알아보았습니다. 
> 다음편에 계속됩니다.

