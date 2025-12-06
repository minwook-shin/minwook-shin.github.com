---
layout: post
title: 코틀린 프로젝트로 안드로이드 Recyclerview adapter에서 BottomSheetDialog를 사용해보기
---

오늘은 코틀린 안드로이드 프로젝트에서 작성된 Recyclerview에 개별적으로 BottomSheetDialog를 띄어보려 합니다.

BottomSheetDialog를 Recyclerview adapter ViewHolder에서 각각의 데이터를 받아 띄우게 출력할 수 있습니다.

## 라이브러리 추가

```java
    implementation 'com.google.android.material:material:1.0.0'
```

기존에 BottomSheetDialog를 추가했던 것처험 androidx 패키지 기준으로 디자인 라이브러리인 material 라이브러리를 의존성에 추가해줍니다.

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
class CFragment: BottomSheetDialogFragment(){
    override fun setupDialog(dialog: Dialog?, style: Int) {
        super.setupDialog(dialog, style)
        val contentView = View.inflate(context, R.layout.fragment_infomation, null)
        dialog?.setContentView(contentView)
    }
}
```

클래스 파일을 만들어서 BottomSheetDialogFragment를 상속받습니다.

## adapter에서 사용

```kotlin
val bottomSheetDialogFragment = CFragment()
bottomSheetDialogFragment.show(supportFragmentManager, bottomSheetDialogFragment.tag)
```

원래 엑티비티 혹은 프래그먼트에서 해당 BottomSheetDialog를 호출할 때에는 위와 같이 작성해야 되지만, adapter에서는 supportFragmentManager가 없습니다.

그래서 RecyclerView Adapter 객체를 쓰는 엑티비티에서 FragmentManager를 인자로 받아와야 합니다.

```kotlin
class RecyclerViewAdapter(val context: Context, val dataSet: ArrayList<Model>,fragmentManager : FragmentManager) : RecyclerView.Adapter<RecyclerView.ViewHolder>() 
```

생성자로 해당 클래스에서 사용할 수 있도록 변수화합니다.

```kotlin
private var mFragmentManager : FragmentManager

init {
    mFragmentManager = fragmentManager
}
```

리스트의 각 개별 항목에 들어갈 data와 FragmentManager를 같이 바인드 해줍니다.

```kotlin
override fun onBindViewHolder(p0: RecyclerView.ViewHolder, p1: Int) {
    val holder: ViewHolder = p0 as ViewHolder
    val data = mDataSet[p1]
    holder.bind(data,mFragmentManager)
}
```

ViewHolder 클래스에서 버튼 클릭 리스너를 만들어 누르면 BottomSheetDialog가 올라오게 작성합니다.

이전에 인자로 받은 mFragmentManager를 이용하여 BottomSheetDialog를 show() 할 수 있습니다.

```kotlin
private inner class ViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView) {
    fun bind(data: Model, fragmentManager: FragmentManager) {
        itemView.button.setOnClickListener {
            val bottomSheetDialogFragment = CFragment(data)
            bottomSheetDialogFragment.show(fragmentManager, bottomSheetDialogFragment.tag)
        }
    }
}
```