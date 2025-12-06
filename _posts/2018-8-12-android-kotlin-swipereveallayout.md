---
layout: post
title: 코틀린으로 작성한 안드로이드 SwipeRevealLayout 라이브러리 사용하여 스와이프 레이아웃 구현하기
---

오늘은 안드로이드 SwipeRevealLayout 라이브러리를 사용하여 스와이프 레이아웃 구현하는 방식을 코틀린으로 작성해보려 합니다.

## 설정

```kotlin
implementation  'com.chauthai.swipereveallayout:swipe-reveal-layout:1.4.1'
```

스와이프 레이아웃 구현하는 라이브러리는 많지만, 이 포스트에서는 swipereveallayout 라이브러리를 사용했습니다.

## 레이아웃

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">
    <data>
    </data>
    <com.chauthai.swipereveallayout.SwipeRevealLayout
        android:id="@+id/swipeLayout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:mode="same_level"
        app:dragEdge="right">
        <LinearLayout
            android:id="@+id/menu"
            android:layout_width="wrap_content"
            android:layout_height="match_parent"
            android:orientation="horizontal"
            android:minWidth="72dp"
            android:background="#main_color"
            android:gravity="center"
            android:clickable="true"
            android:focusable="true">
            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:textColor="#ffffff"
                android:gravity="center"
                android:textSize="15sp"
                android:textAlignment="gravity"
                android:text="menu"/>
        </LinearLayout>
        <LinearLayout
            android:id="@+id/front_wrapper"
            android:layout_width="match_parent"
            android:layout_height="wrap_content">
            <ImageView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:src="@drawable/photo"
                android:maxHeight="24dp"
                android:maxWidth="24dp"
                android:adjustViewBounds="true"/>
            <TextView
                android:id="@+id/enter_text"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="text"/>
            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:gravity="right"/>
        </LinearLayout>
    </com.chauthai.swipereveallayout.SwipeRevealLayout>
</layout>
```

SwipeRevealLayout 라이브러리의 모드에는 same_level과 normal이 있으며, dragEdge는 상하좌우로 설정가능합니다.

SwipeRevealLayout 태그로 감싼 후에 밀면 나오는 화면을 먼저 레이아웃으로 만들어주고, 그 다음에 원래 보이는 화면을 레이아웃으로 만들어줍니다.

그리고 이 SwipeRevealLayout들이 모여있을 RecyclerView를 넣을 엑티비티에 RecyclerView를 널어줍니다.

```xml
<android.support.v7.widget.RecyclerView
        android:id="@+id/mRecyclerView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"   
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="parent" />
</android.support.constraint.ConstraintLayout>
```

## adapter

```kotlin
class RecyclerViewAdapter(context : Context, dataSet:ArrayList<String>) : RecyclerView.Adapter<RecyclerView.ViewHolder>()
```

RecyclerView 어댑터 클래스를 만들어줍니다.

```kotlin
    lateinit var itemBinding:ViewListItemBinding

    override fun onCreateViewHolder(p0: ViewGroup, p1: Int): RecyclerView.ViewHolder {
        val layoutInflater = LayoutInflater.from(p0.context)
        itemBinding = DataBindingUtil.inflate(layoutInflater,R.layout.view_list_item, p0, false)
        return ViewHolder(itemBinding.root.rootView)
    }
```

데이터 바인드를 위해 onCreateViewHolder 메소드를 오버라이드하고 ViewHolder 클래스로 반환합니다.

```kotlin
    override fun onBindViewHolder(p0: RecyclerView.ViewHolder, p1: Int) {
        val holder: ViewHolder = p0 as ViewHolder

        if (0 <= p1 && p1 < dataSet.size) {
            val data = dataSet[p1]

            private val binderHelper = ViewBinderHelper()
            binderHelper.bind(itemBinding.swipeLayout, data)
            binderHelper.setOpenOnlyOne(true)
            holder.bind(data)
        }
    }
```

onBindViewHolder 메소드로 데이터와 ViewHolder를 엮어주면서 ViewBinderHelper 객체로 setOpenOnlyOne 속성처럼 하나만 열리는 등 옵션을 선택할 수 있습니다.

```kotlin
    override fun getItemCount(): Int {
        return dataSet.size
    }
```

getItemCount 메소드로 데이터의 갯수를 반환해줍니다.

```kotlin
    private inner class ViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView) {
        fun bind(data: String) {
            itemBinding.enterText.text= data
        }
    }
}
```

inner class로 ViewHolder를 만들어주어 뷰와 데이터를 맵핑해줍니다. 

## data

```kotlin
val array = ArrayList<String>()
array.add("text 1")
array.add("text 2")
array.add("text 3")
array.add("text 4")
array.add("text 5")
```

RecyclerViewAdapter에 넣을 데이터를 임시로 만들어줍니다.

## main

```java
bind.mRecyclerView.layoutManager = LinearLayoutManager(activity)
var adapter: RecyclerViewAdapter? = RecyclerViewAdapter(activity!!.applicationContext, userArray)
bind.mRecyclerView.adapter = adapter
bind.mRecyclerView.setHasFixedSize(true)
```

layoutManager를 설정하고, 이전에 만든 ArrayList를 어댑터에 넣어줍니다.

구현한 RecyclerViewAdapter도 객체로 만들어서 RecyclerView에 넣어줍니다.

activity!!.applicationContext는 이 코드가 사용된 곳이 fragment라서 쓰인 코드이므로, 일반적인 activity라면 바로 context를 불러오면 됩니다.

이제 빌드를 진행하면 Recycler View가 출력되는 것을 볼 수 있습니다.