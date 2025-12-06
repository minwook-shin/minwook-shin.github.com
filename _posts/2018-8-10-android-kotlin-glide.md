---
layout: post
title: 코틀린으로 작성한 안드로이드 glide 라이브러리 사용해보기
---

오늘은 안드로이드 라이브러리인 glide를 코틀린 확장 함수로 작성해서 사용해보려 합니다.

추가로 바인딩 어댑터로도 실습해보려 합니다.

## 설정

```java
implementation 'com.github.bumptech.glide:glide:4.7.1'
annotationProcessor 'com.github.bumptech.glide:compiler:4.7.1'
```

glide 라이브러리를 gradle에 추가해줍니다.

## 확장 함수

```kotlin
fun ImageView.loadBitmap(image: Bitmap?) {
        Glide.with(this).load(imageUrl).into(this)
    }
```

이와 같이 ImageView에 loadBitmap라는 확장 함수를 추가해놓는다면, findviewbyid를 해서 찾던지 Data bind를 해서 해당 ImageView 객체에서 바로 온점(.)을 찍고
직접 만든 함수를 쓸 수 있습니다.

```kotlin
fun ImageView.loadUrl(imageUrl: String?) {
        Glide.with(this).load(imageUrl).into(this)
    }
```

마찬가지로 String타입의 url도 가능합니다.

## 바인딩 어댑터

```kotlin
@BindingAdapter("setBitmap")
fun setBitmap(view: ImageView,bitmap:Bitmap?){
    bitmap?.let {
        Glide.with(view.context).load(bitmap).into(view)
    }
}
```

바인딩 어댑터로 바꿔서 적용하면 방금 만든 메소드의 내용에서 this를 ImageView 타입의 view의 context로 넣어주면 됩니다.

## xml

```xml
<data>
    <variable
        name="bitmap"
        type="android.graphics.Bitmap"/>
</data>
```

바인딩 어댑터를 사용한다면 data 태그에 Bitmap 타입의 변수를 만들어주어야 합니다.

```xml
<ImageView
        android:id="@+id/img"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="parent"
        app:setBitmap="@{bitmap}"/>
```

아까 만든 바인딩 어댑터와 변수를 준비해서 ImageView를 만들 때 속성으로 넣어주면 됩니다.

소개는 확장함수와 바인딩 어댑터 두개 다 했지만, 실제로 코드에서는 하나만 선택하여 작성하면 됩니다.