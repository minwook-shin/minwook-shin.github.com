---
layout: post
title: 코틀린 프로젝트로 안드로이드에서 BottomSheetDialog를 쉽게 사용해보기
---

오늘은 코틀린 안드로이드 프로젝트에서 BottomSheetDialog를 쉽게 사용해보려 합니다.

주로 하단에서 모달을 올릴 때에 사용하는 방법입니다.

## 라이브러리 추가

```java
    implementation 'com.google.android.material:material:1.0.0'
```

androidx 패키지 기준으로 디자인 라이브러리인 material 라이브러리를 의존성에 추가해줍니다.

## 레이아웃

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal" android:layout_width="match_parent"
    android:layout_height="wrap_content">
    <com.google.android.material.textfield.TextInputLayout
        android:id="@+id/chatBox"
        style="@style/Widget.MaterialComponents.TextInputLayout.OutlinedBox"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_weight="0.9">

        <com.google.android.material.textfield.TextInputEditText
            android:id="@+id/chatEditText"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:inputType="textPersonName"
            android:singleLine="true"
            android:fontFamily="@font/nanum_gothic_regular"/>
    </com.google.android.material.textfield.TextInputLayout>
</LinearLayout>
```

하단에서 나타날 레이아웃을 준비해줍니다.

예제는 material 디자인의 텍스트 입력창만 추가한 모습입니다.

## 클래스

```kotlin
class InformationFragment: BottomSheetDialogFragment(){
    override fun setupDialog(dialog: Dialog?, style: Int) {
        super.setupDialog(dialog, style)
        val contentView = View.inflate(context, R.layout.fragment_infomation, null)
        dialog?.setContentView(contentView)
    }
}
```

클래스 파일을 만들어서 BottomSheetDialogFragment를 상속받습니다.

이제 해당 BottomSheetDialog를 호출할 엑티비티 혹은 프래그먼트에서 아래와 같이 작성합니다.

```kotlin
val bottomSheetDialogFragment = InformationFragment()
bottomSheetDialogFragment.show(supportFragmentManager, bottomSheetDialogFragment.tag)
```

```kotlin
val bottomSheetDialogFragment = InformationFragment()
bottomSheetDialogFragment.show(activity?.supportFragmentManager, bottomSheetDialogFragment.tag)
```

만약 프래그먼트라면 위와 같이 엑티비티를 불러와서 supportFragmentManager를 가져옵니다.

이제 앱을 실행시켜서 원하는 부분에서 BottomSheetDialog를 호출해보면 됩니다.

## 이벤트 리스너

```kotlin
val button = contentView.findViewById(R.id.sendButton) as Button
button.setOnClickListener {
    this.dismiss()
}
```

추가로 적어보자면, BottomSheetDialogFragment를 상속받은 클래스의 뷰에서 id를 찾아 버튼이나 입력 바 등에 사용되는 이벤트 리스너를 만들 수 있습니다. 