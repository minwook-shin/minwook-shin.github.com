---
layout: post
title: 코틀린으로 안드로이드 Custom View에 이벤트 리스너를 바인딩 어뎁터로 넘겨주는 방법에 대하여 알아보기
---

오늘은 안드로이드에서 custom view를 만들었을 때 해당 클래스에서만 onclick되는 것을 커스텀 뷰를 사용할 때마다 activity에서 바인드 어댑터를 거쳐 이벤트를 전달해주는 방법을 포스팅하려 합니다.

이번 포스트에서도 데이터 바인딩으로 작동하는 코드가 포함되어 있으며, activity가 아닌 fragment에서 이벤트를 넘겨주는 상황으로 예시 코드가 작성되었습니다.

## custom view xml

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

## custom view class

```kotlin
class ListItemView: BaseView, View.OnClickListener
```

커스텀 뷰 클래스를 만드는데에는 ConstraintLayout를 상속받은 BaseView 클래스가 필요하며, 클릭 이벤트 리스너가 필요하다면 OnClickListener도 가져와 상속받습니다.

```kotlin
    interface eventListener{
        fun onChangeClick(view: View)
    }
```

인터페이스를 만들어주면서 인자는 view를 받아 해당 뷰의 정보를 사용할 수 있게 해줍니다.

```kotlin
    private var listener: eventListener? = null
```

전달받은 이벤트를 담을 변수를 방금 만든 인터페이스 타입으로 합니다.


```kotlin
    constructor(context: Context?,listener:eventListener,text:String?,tag:Any) : super(context)
    {
        bind = DataBindingUtil.inflate(LayoutInflater.from(context), R.layout.view_user_list_item, this, true)
        bind.text = this.text
        bind.text2 = this.text2
        this.text.set(tag as String)
        this.text2.set(text)
        this.listener = listener
        this.tag = tag

        initEvents()

    }
```

ConstraintLayout를 상속받은 클래스에서는 constructor가 3개가 있었습니다.

하지만, 이번 커스텀 뷰에 필요한 인자들이 더 있으므로, 필요한 인자는 constructor를 하나 더 만들어서 인자로 받습니다.

이 constructor 역시 데이터바인딩을 사용할 수 있습니다.

위 예시 코드는 텍스트를 텍스트뷰에 출력하며 이벤트 리스너를 커스텀 뷰로 넘겨줍니다.

```kotlin
    private fun initEvents()
    {
        bind.bottomWrapper.setOnClickListener(this)
    }
```

이벤트 리스너를 this로 설정하며 해당 클래스에서 오버라이드한 onClick 메소드와 연결해줍니다.

```kotlin
    override fun onClick(v: View) {
        listener?.onChangeClick(this)
    }
}
```

onClick에서 오는 view 인자를 사용하지 않고, this를 써야지 전체 view에 대한 이벤트를 가져올 수 있습니다.

## fragment

```kotlin
val listener = object : ListItemView.eventListener{
        override fun onChangeClick(view: View) {
            view.tag?.let { App.INSTANCE.context().longToast(it as String) }
        }
}

val array = mutableMapOf<String,String>()

array["사용자 1"] = "관리자"
array["사용자 2"] = "사용자"
array["사용자 3"] = "사용자"
array["사용자 4"] = "사용자"
array["사용자 5"] = "사용자"

bind.items = userArray
bind.listener = listener
```

listener에는 ListItemView 클래스의 eventListener 인터페이스를 구현해서 넣어줍니다.

그리고 텍스트뷰에 표현할 텍스트를 Map의 키로 쓸 것이기 때문에 Map을 아이템으로 넣어줍니다.

## binding adpapter

```kotlin
@BindingAdapter("Item","Listener")
    @JvmStatic
    fun setUserItem(view: LinearLayout,item : MutableMap<String,String>,listener:UserListItemView.eventListener){
        view.removeAllViews()

        for (it in item.keys){
            view.addView(ListItemView(view.context,listener,item[it],it as Any))
        }
    }
```

바인딩 어댑터를 만들어줍니다.

LinearLayout의 속성에서 Item과 Listener를 사용하여 두가지의 인자를 같이 받을 수 있습니다.

view를 초기화해주고 addView로 커스텀 뷰를 화면에 출력하게 해줍니다.

## fragment xml

```xml
    <data>
        <variable
            name="items"
            type="java.util.Map&lt;String,String&gt;"/>
        <variable
            name="listener"
            type="com.zerofounders.garage.view.List.UserListItemView.eventListener"/>
    </data>
```

방금 bind.items와 bind.listener에 넣었던 값은 이 곳에 들어와서 바로 뷰에 보여지게 됩니다.

```xml
<LinearLayout
    android:id="@+id/UserList"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    Item="@{items}"
    Listener="@{listener}">
</LinearLayout>
```

방금 만든 바인딩 어댑터로 만든 속성에 바인드된 값들을 넣어줍니다.


## build

이전 포스트와 동일하게 모든 준비가 끝나면 LinearLayout에 addview 메소드로 커스텀 뷰를 추가하고 안드로이드 스튜디오로 빌드를 수행하면 됩니다.