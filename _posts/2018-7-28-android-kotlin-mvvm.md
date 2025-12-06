---
layout: post
title: 코틀린으로 작성한 안드로이드 MVVM 패턴 사용하기
---

오늘은 안드로이드 mvvm 패턴을 코틀린으로 간단하게 작성해보았습니다.

mvvm 패턴은 model과 view, 그리고 view model로 이루어져 있습니다.

DataBinding 라이브러리를 같이 사용하는 패턴입니다.

아래는 간단한 문자열을 출력하고, 버튼을 누르면 값이 변경되는 예시입니다.

## model

```kotlin
data class Test(var test_text : String = "null")
```

데이터들의 집합을 데이터 클래스로 구현합니다.

## view model

```kotlin
class TestViewModel{

    private val testText = Test()
    val hello = ObservableField<String>(testText.test_text)

    fun addText(){
        hello.set("add text!")
    }

}
```

model과 연결하고, ObservableField 데이터를 만들어줍니다.

만약 관련 함수를 구현하려고 하면 view model에서 구현하고 view에 연결해주면 됩니다.

## view

```xml
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
            android:text="@{model.hello}" />

        <Button
            android:id="@+id/button"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Button"
            app:layout_constraintEnd_toStartOf="@+id/hell_world"
            app:layout_constraintStart_toStartOf="@+id/hell_world"
            app:layout_constraintTop_toBottomOf="@+id/hell_world"
            android:onClick="@{() -> model.addText()}"/>

    </android.support.constraint.ConstraintLayout>
</layout>
```

보이는 화면을 구현하기 위해 xml 파일을 만들어줍니다.

데이터 바인딩을 적용하기 위해 layout 태그로 감쌌으며, data 태그에서 view model을 가져와서 ObservableField와 연결합니다.

view model에 구현한 함수들을 android:onClick 속성으로 연결할 수 있습니다.

```kotlin
class MainActivity : BaseActivity() {

    private val bind by lazy{DataBindingUtil.setContentView<ActivityMainBinding>(this,R.layout.activity_main)}

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        bind
        bind.model = TestViewModel()
    }
}
```

Activity 클래스에서는 DataBindingUtil을 이용한 데이터 바인딩을 하게 됩니다.

```bind.model = TestViewModel()```처럼 view model을 연결합니다.

연결을 했으므로 ObservableField 타입의 데이터가 변화할 때마다 뷰에 반영되게 됩니다.

