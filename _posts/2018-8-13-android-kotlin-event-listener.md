---
layout: post
title: 코틀린으로 작성한 안드로이드 Event Listener 알아보기
---

오늘은 기존에 자바로 작성하던 이벤트 리스너를 코틀린으로 변환하여 만들어보려 합니다.

## OnclickListener 인터페이스 구현

클래스에 인터페이스를 상속받아서 구현하는 방식입니다.

자바와 별 다를 바 없이, 코틀린의 문법이 차이 날 뿐 입니다.

```kotlin
class ClassName: BaseView, View.OnClickListener
```

```kotlin
override fun onClick(v: View) {
    //method
}
```

클래스에 onClick 메소드를 오버라이딩하면 됩니다.

## listener 객체 전달

listener 객체를 만들어서 OnClickListener에 전달하는 방식입니다.

```kotlin
val listener = object : View.OnClickListener{
    override fun onClick(p0: View?) {
        //method
    }
}

button.setOnClickListener(listener)
```

## 익명 객체 전달

```java
button.setOnClickListener(object :View.OnClickListener{
    override fun onClick(p0: View?) {
        //method
    }
})
```

버튼의 OnClickListener에 익명 객체를 바로 사용합니다.

onClick 메소드를 오버라이딩하여 사용합니다.

## lambda

익명 객체로 전달하는 방식은 IDE에서 자체적으로 Convert to lambda라고 밑줄이 그어집니다.

간단해서 가장 많이 쓰이는 방식이기도 합니다.

```java
button.setOnClickListener {
    //method
}
```