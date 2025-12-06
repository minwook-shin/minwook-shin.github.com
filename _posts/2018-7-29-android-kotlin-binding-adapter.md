---
layout: post
title: 코틀린으로 작성한 안드로이드 바인딩어댑터 사용하기
---

오늘은 안드로이드에서 데이터 바인딩을 적용하여 바인딩어댑터를 코틀린으로 사용해보려 합니다.

## 설정

기존에 data binding을 적용했던 빌드 설정을 하시면 됩니다.

```xml
apply plugin: 'kotlin-kapt'
```

```java
android {
    dataBinding {
        enabled true
    }
}
```

```java
dependencies {
kapt 'com.android.databinding:compiler:3.1.3'
}
```

kotlin-kapt 플러그인을 추가해주고, dataBinding을 활성화해줍니다.

그리고 dependencies에 "kapt 'com.android.databinding:compiler:3.1.3'"를 추가해줍니다.

위 예시에 나온 데이터 바인딩 컴파일러 버전 3.1.3는 자신의 gradle 버전과 맞추어야 한다고 합니다.

## binding adapter

```kotlin
object BindingAdapter{
    @BindingAdapter("text")
    @JvmStatic
    fun setText(view : TextView, text :  String){
        view.text = text+text 
        }
}
```

@BindingAdapter으로  이 함수가 바인딩 어댑터라고 선언해주고, 자바에서 static 키워드를 대체하기 위해 @JvmStatic를 추가해줍니다.

함수의 첫 인자는 적용한 컴포넌트 뷰를 작성해주고, 두번째 인자는 데이터 바인딩으로 인해 들어오는 값을 적어줍니다.


## layout 설정

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">

    <data>
        <variable
            name="model"
            type="com.zerofounders.garage.ViewModel.TestViewModel"/>
    </data>

    <android.support.constraint.ConstraintLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".View.MainActivity">

        <TextView
            android:id="@+id/hell_world"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintLeft_toLeftOf="parent"
            app:layout_constraintRight_toRightOf="parent"
            app:layout_constraintTop_toTopOf="parent"
            text="@{model.hello}" />

    </android.support.constraint.ConstraintLayout>
</layout>

```

기존의 레아이웃과 별 다를 바가 없긴 하지만, 하나 다른 것은 ```<layout>``` 태그로 감싼다는 것입니다.

layout으로 기존의 코드를 감싸면 데이터바인딩을 사용할 수 있는 레이아웃이 됩니다.

그리고 data 태그를 통해 바인드한 객체의 값을 binding adapter로 보내 연산할 준비를 합니다.

참고로 model.hello는 "hello, world!"라는 문자열로 초기화되어 있는 model 클래스의 값을 ObservableField로 준비하고 있는 변수입니다.

코딩을 마치고 빌드를 성공적으로 수행하게 되면, 앱에는 "hello, world!"가 출력되게 됩니다.
