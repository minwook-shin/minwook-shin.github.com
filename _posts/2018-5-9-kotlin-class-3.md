---
layout: post
title: Kotlin의 class에 대하여 알아보기 3
---

오늘은 코틀린에서의 접근 권한에 대하여 알아보겠습니다.

## 접근 권한

private은 접근 범위가 현재 클래스에 한정되고, protected는 현재 클래스와 상속 받은 클래스, internal는 모듈만 접근할 수 있습니다.

당연히 public은 접근범위가 모든 범위이며, 접근 지정자를 지정하지 않을 경우에는 기본적으로 public입니다.

보통은 private과 public을 주로 사용한다고 합니다.

## private

private을 적용한 예시로는 아래와 같으며, private으로 클래스안에서 보호할 수 있습니다.

```java
interface Itest{
    fun TestInterface()
}

private class test():Itest{
    private var name : String = "test"
    private var a : Int = 1
    private var b : Int = 2
    fun sum():Int{
        return a + b
    }
    override fun TestInterface(){
        println("interface")
    }
}

fun main(args: Array<String>) {
    var real = test()
    var result = real.sum();
    println(result)
//     println(real.name)
}
```

## protected

protected를 적용한 예시로는 아래와 같습니다.

상속받지 못한 함수에서는 값을 읽지 못합니다.

```java
interface Itest{
    fun TestInterface()
}

open class test():Itest{
    open var name : String = "test"
    protected var a : Int = 1
    protected var b : Int = 2
    open fun sum():Int{
        return a + b
    }
    override fun TestInterface(){
        println("interface")
    }
}

class test2() : test(){
    override var name : String = "test2"
}

fun main(args: Array<String>) {
    var real = test2()
    var result = real.sum();
    println(result)
//     println(real.a)
}
```

## internal

internal을 적용한 예시는 아래와 같습니다.

이전의 private과 protected은 상속받지 못한 다른 함수에서 접근하지 못했지만, internal 접근 지정자는 모듈내로는 자유롭게 접근가능 합니다.

```java
interface Itest{
    fun TestInterface()
}

open class test():Itest{
    open var name : String = "test"
    internal var a : Int = 1
    internal var b : Int = 2
    open fun sum():Int{
        return a + b
    }
    override fun TestInterface(){
        println("interface")
    }
}

class test2() : test(){
    override var name : String = "test2"
}

fun main(args: Array<String>) {
    var real = test2()
    var result = real.sum();
    println(result)
    println(real.a)
    println(real.b)
}
```

## public

public을 적용한 예시는 아래와 같으며, public이라고 지정하지 않아도 자동으로 public으로 처리되는 특징이 있습니다.

모두에게 열려있는 접근 지정자입니다.

```java
public class test(){
    var name : String = "test"
    public var a : Int = 1
    public var b : Int = 2
    public fun sum():Int{
        return a + b
    }
}

fun main(args: Array<String>) {
    var real = test()
    var result = real.sum();
    println(result)
    println(real.a)
    println(real.b)
}
```