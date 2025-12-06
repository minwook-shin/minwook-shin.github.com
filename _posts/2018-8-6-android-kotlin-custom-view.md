---
layout: post
title: 코틀린으로 안드로이드에서 Custom View 만들기
---

오늘은 안드로이드에서 커스텀 뷰를 코틀린과 데이터 바인딩으로 구현해보려 합니다.

## base view

우선 기반이 되는 파일을 만든 것이 좋습니다.

커스텀 뷰를 만들 때에는 생성자로부터 초기화나 값 삽입등을 하기 때문에 베이스가 중요합니다.

```kotlin
open class BaseView: ConstraintLayout {
    constructor(context: Context?) : super(context)
    constructor(context: Context?, attrs: AttributeSet?) : super(context, attrs)
    constructor(context: Context?, attrs: AttributeSet?, defStyleAttr: Int) : super(context, attrs, defStyleAttr)

    override fun onDetachedFromWindow() {
        super.onDetachedFromWindow()
    }
}
```

## 디자인 

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">
    <data>
        <import type="android.view.View"/>
        <variable
            name="text"
            type="android.databinding.ObservableField&lt;String&gt;"/>
        <variable
            name="text2"
            type="android.databinding.ObservableField&lt;String&gt;"/>
    </data>
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content">
            <ImageView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:src="@drawable/img_photo"
                android:maxHeight="24dp"
                android:maxWidth="24dp"
                android:adjustViewBounds="true"/>
            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="@{text}"/>
            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:text="@{text2}"
                android:gravity="right"/>
        </LinearLayout>
</layout>
```

ImageView와 TextView로 이루어진 커스텀 뷰 xml을 만들어 줍니다.

데이터 바인딩을 할 수 있는 형식이므로 data 태그에서 값을 가져올 수도 있습니다.

위 예시 코드는 ObservableField<String> 타입의 변수와 연결하므로 type에는 ```"android.databinding.ObservableField&lt;String&gt;"```라고 적어줍니다.

## 클래스

```kotlin
class UserListItemView: BaseView, View.OnClickListener{
    var bind : ViewUserListItemBinding
    var text = ObservableField<String>()
    var text2 = ObservableField<String>()

    constructor(context: Context?) : super(context)
    {
        bind = DataBindingUtil.inflate(LayoutInflater.from(context), R.layout.view_list_item, this, true)
        bind.text = text
    }
    constructor(context: Context?, attrs: AttributeSet?) : super(context, attrs)
    {
        bind = DataBindingUtil.inflate(LayoutInflater.from(context), R.layout.view_list_item, this, true)
        bind.text = text
    }
    constructor(context: Context?, attrs: AttributeSet?, defStyleAttr: Int) : super(context, attrs, defStyleAttr)
    {
        bind = DataBindingUtil.inflate(LayoutInflater.from(context), R.layout.view_list_item, this, true)
        bind.text = text
    }

    constructor(context: Context?,text:String?,tag:Any) : super(context)
    {
        bind = DataBindingUtil.inflate(LayoutInflater.from(context), R.layout.view_list_item, this, true)
        bind.text = this.text
        bind.text2 = this.text2
        this.text.set(tag as String)
        this.text2.set(text)
        this.tag = tag
    }
    override fun onClick(v: View) {
    }
}
```

기존 ConstraintLayout 클래스를 상속받은 BaseView와 다르게 필요한 인자들로 구성된 생성자를 만들어 줍니다.

커스텀 뷰에 들어갈 값이나, 이벤트, 태그들이 주로 필요한 인자에 들어갑니다.

생성자에서는 DataBindingUtil을 이용하여 레이아웃과 연결해주고 바인딩을 해주면서 값을 전달해둡니다.

onClick을 오버라이드하여 커스텀 뷰에서 각각의 컴포넌트들이 클릭되었을 때의 이벤트들을 수신합니다.