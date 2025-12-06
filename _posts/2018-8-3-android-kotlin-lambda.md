---
layout: post
title: 코틀린으로 안드로이드에서 간단한 람다 사용하기
---

오늘은 안드로이드에서 코틀린으로 작성된 람다를 간단히 알아보려 합니다.

## 개요

이벤트에 따른 핸들러를 실행하는 것을 추구하는 코드들은 변수에 값을 저장하거나 다른 함수로 넘겨야 될 경우가 많습니다.

이는 람다식을 이용하면 간단하게 코딩할 수 있으며, 이를 이용하면 함수를 선언할 필요가 없고, 코드 블럭 자체를 직접 함수에 전해줄 수 있습니다.

## 안드로이드 예제

```java
buttonId.setOnClickListener(new OnClickListener (){
    @Override
    public void onClick(View v){
        //code
    }
});
```

보통 위와 같이 단순한 마우스 클릭 이벤트를 수신받는 이벤트 리스너도 이렇게 중복되는 코드들이 있습니다.

나중에 코드의 양이 늘어나면 다소 복잡해질 수 있습니다.

```java
buttonId.setOnClickListener{ //code 
}
```

같은 역헐을 하지만 훨씬 간결해졌습니다.

## 파라미터

```kotlin
val sum = {a:Int, b:Int -> a+b}
```

파라미터와 식은 같은 중괄호에 있고, 단지 화살표로 구별됩니다.


```java
buttonId.setOnClickListener { it ->
    //code
}
```

파라미터가 하나밖에 없고 타입을 컴파일러가 바로 추론할 수 있다면 위와같이 it를 명시할 필요는 없습니다.