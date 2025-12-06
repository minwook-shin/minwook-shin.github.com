---
layout: post
title: 코틀린으로 작성한 안드로이드 onFocusChangeListener Event Listener 사용하기
---


## 자바 코드

```java
item.setOnFocusChangeListener(new View.OnFocusChangeListener() {
    @Override
    public void onFocusChange(View v, boolean hasFocus) {
        if(hasFocus) {
            //method
        }
});
```

OnFocusChangeListener에 onFocusChange 메소드를 오버라이드하여 hasFocus이라는 boolean 타입의 변수로 포커스를 판단할 수 있습니다.



## 코틀린 코드

위 자바 코드대로 비슷하게 작성하려면

```java
item.setOnFocusChangeListener(object :OnFocusChangeListener{
    override fun onFocusChange(v: View?, hasFocus: Boolean) {
    }
})
```

위와 같이 OnFocusChangeListener 타입의 object를 만들 수는 있습니다.

다만, 위와 같이 코딩을 하면 Use property access syntax라면서 프로퍼티 접근 문법을 사용하라고 경고합니다.

```java
item.onFocusChangeListener = object :OnFocusChangeListener{
    override fun onFocusChange(v: View?, hasFocus: Boolean) {

    }
}
```

경고를 없애기 위해 setOnFocusChangeListener를 onFocusChangeListener으로 프로퍼티 접근 문법을 사용했습니다.

그런데 Convert to lambda라며 약한 경고가 또 발생합니다.

그래서 람다식으로 바꾸어보았습니다.

```java
item.onFocusChangeListener = OnFocusChangeListener { v, hasFocus -> }
```

OnFocusChangeListener을 람다식으로 자동 변환된 모습입니다.

```java
item.onFocusChangeListener = OnFocusChangeListener { view, hasFocus ->
    if (hasFocus){
        //method
    } 
}
```

자동 변환된 람다식에 포커스에 따른 변화를 주기 위해서 조건문을 추가하면 위와 같은 예제 코드가 됩니다.

최종적으로 작성된 예제코드를 보면, 자바에서는 익명 객체를 전달하는데에 그쳤지만 코틀린에서는 코틀린스럽게 처리할 수 있습니다.