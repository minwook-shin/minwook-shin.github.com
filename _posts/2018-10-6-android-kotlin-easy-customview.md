---
layout: post
title: 코틀린 프로젝트로 안드로이드에서 커스텀뷰를 쉽게 사용해보기
---

오늘은 코틀린 안드로이드 프로젝트에서 커스텀뷰를 쉽게 사용해보려 합니다.

## 커스텀뷰 구현

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="horizontal"
    android:padding="@dimen/dp_10">
    <ImageView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/ic_exit_to_app_black_24dp"
        android:minHeight="@dimen/dp_40"
        android:minWidth="@dimen/dp_40"
        android:layout_marginEnd="@dimen/dp_10" />
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="text"
        android:textSize="@dimen/sp_16"
        android:layout_marginTop="10dp"/>
</LinearLayout>
```

우선 커스텀뷰에 보일 안드로이드 위젯 구조를 잡아야합니다.

xml로 작성합니다.

```kotlin
import android.content.Context
import android.util.AttributeSet
import android.widget.LinearLayout

open class BaseView(context: Context, attrs: AttributeSet? = null, defStyleAttr: Int = 0) : LinearLayout(context, attrs, defStyleAttr)
```

BaseView를 만들어서 하위 커스텀뷰에 일괄적으로 적용할 수 있도록 구현합니다.

LinearLayout를 상속받습니다.

```kotlin
class CustomvView @JvmOverloads constructor(context: Context, attrs: AttributeSet? = null, defStyleAttr: Int = 0) : BaseView(context, attrs, defStyleAttr) {
    init {
        LayoutInflater.from(context).inflate(R.layout.custom_button, this, true)
    }
}
```

미리 구현한 BaseView를 가져다와서 구현하고자 하는 xml 디자인 파일을 inflate해줍니다.

## 커스텀뷰 사용

```xml
<?xml version="1.0" encoding="utf-8"?>

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#ffffff"
    tools:layout_editor_absoluteY="81dp"
    android:orientation="vertical">

    <io.github.package.view.custom.CustomvView
        android:id="@+id/logoutButton"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@drawable/button_shadow"
        android:clickable="true"
        android:focusable="true"
        android:elevation="4dp"
        android:layout_margin="10dp"/>
</LinearLayout>
```

위와 같이 기존 위젯 클래스처럼 직접사용할 수 있고, 엑티비티에서 addview()로 커스텀뷰를 추가할 수도 있습니다.