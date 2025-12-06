---
layout: post
title: 코틀린으로 안드로이드에서 커스텀 다이얼로그 만들어서 사용하기
---

오늘은 안드로이드에서 커스텀 다이얼로그를 만드는 방법을 코틀린으로 작성해보려 합니다.

## 설정

우선 이번 방법은 데이터 바인딩을 이용합니다.

```java
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

## 레이아웃

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">

    <data>
        <variable
            name="title"
            type="String" />
        <variable
            name="massage"
            type="String" />
        <variable
            name="okay"
            type="String" />
    </data>
```

우선 데이터바인딩으로 하기 위해 layout 태그로 감싸고 variable 태그를 이용해 클래스에서 가져오고 싶은 변수의 이름을 정해줍니다.

view

```xml
    <RelativeLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="#00ffffff">
```

그리고 다이얼로그의 뒷편을 투명한 레이아웃을 만들어줍니다.

```xml
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_centerInParent="true"
            android:layout_margin="40dp"
            android:background="#ffffff"
            android:elevation="25dp"
            android:gravity="center"
            android:orientation="vertical">
            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginBottom="12dp"
                android:layout_marginTop="17dp"
                android:text="@{title}"
                android:textSize="20sp"
                android:textStyle="bold"/>
            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginBottom="13dp"
                android:text="@{massage}"
                android:textSize="16sp" />
```

여기까지는 다이얼로그의 타이틀과 메시지를 보여주는 부분이며, LinearLayout에서 vertical으로 되어있어서 차례대로 내려가보입니다.

```xml
            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:orientation="horizontal">
                <RelativeLayout
                    android:id="@+id/okay_button"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    android:clickable="true"
                    android:focusable="true"
                    android:background="@drawable/dialog_button">
                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:layout_centerInParent="true"
                        android:layout_marginBottom="16dp"
                        android:text="@{okay}"
                        android:textSize="14sp" />
                </RelativeLayout>
            </LinearLayout>
        </LinearLayout>

    </RelativeLayout>
</layout>
```

레이아웃을 버튼으로 만들기 위해서는 clickable 속성을 부여해주면 확인 버튼을 만들 수 있습니다.

clickable 속성이 true로 되면 이벤트 리스너에서 이벤트를 수신할 수 있습니다.

## 클래스

```kotlin
class CustomDialog(context:Context) : Dialog(context), View.OnClickListener
```

클래스는 다이얼로그 클래스와 클릭 이벤트 리스너를 상속받으면서 작업합니다.

context는 다이얼로그 상위 클래스로 넘겨줍니다.

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    setCanceledOnTouchOutside(false)
    window.setBackgroundDrawable(ColorDrawable())
    window.setDimAmount(0.0f)
    bind.title = this.title
    bind.massage = this.msg
    bind.okay = this.okay

    bind.okayButton.setOnClickListener(this)
    setContentView(bind.root)
}

class Builder(context: Context){
    private val dialog = CustomDialog(context)
    fun setTitle(text:String): Builder {
        dialog.title = text
        return this
    }
    fun setMassage(text:String): Builder {
        dialog.msg = text
        return this
    }
    fun setOkayButton(text:String): Builder {
        dialog.okay = text
        return this
    }
    fun show(): CustomDialog {
        dialog.show()
        return dialog
    }
}
override fun onClick(p0: View?) {
}
```

onCreate 메소드를 오버라이드하여 공백을 터치하면 다이얼로그가 해제되지 않게 해주고 배경을 원래 의도처럼 투명하게 하기 위해서 옵션을 메소드로 설정해줍니다.

Builder 클래스를 작성해서 Builder로 들어온 값을 데이터 바인딩으로 xml에 값이 전달되도록 합니다.

## 사용

```kotlin
private fun makeDialog(){
        CustomDialog.Builder(this)
                .setMode(HorizontalDialog.MODE_OK_CANCEL)
                .setTitle("제목")
                .setMassage("메시지")
                .setOkayButton("예")
                .show()
}
```

이제 엑티비티에서는 아까 만들었던 Builder 클래스를 불러와서 값을 넣어줍니다. Builder는 this를 반환하기 때문에 온점으로 연결해서 한번에 CustomDialog로 적용시킬 수 있습니다.

코딩을 마치고 안드로이드 스튜디오로 빌드를 하면 커스텀 다이얼로그가 화면에 나타나게 됩니다.

버튼에 대한 이벤트를 부여하기 위해서는 CustomDialog 클래스를 만들 때에 오버라이드한 onClick 메소드를 활용해야 합니다.