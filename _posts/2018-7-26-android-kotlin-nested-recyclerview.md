---
layout: post
title: 코틀린으로 작성한 안드로이드 recycler view 중첩해서 사용하기
---

오늘은 recycler view 안에 recycler view가 들어있는, 즉 recycler view를 중첩해서 사용하는 방식을 알아보려합니다.

보통 recycler view는 xml과 activity가 adapter로 연결되어 있어서 쉽게 구현할 수 있습니다.

하지만 중첩된 recycler view는 잠깐 생각해보면 recycler view 안에 layout이 하나 더 있는 것입니다.

분명 activity에서는 중첩된 recycler view를 찾기 힘들지 않나라는 생각이 들겁니다.

하지만, 결론부터 말씀드리자면 Holder에서 adapter로 연결해주면 됩니다.

## 상위 xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">

    <data>
    </data>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@drawable/border"
        android:orientation="vertical">

        <TextView
            android:id="@+id/list_text"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:textAppearance="@style/TextAppearance.AppCompat.Large"
            android:hint="@string/text"/>

        <android.support.v7.widget.RecyclerView
            android:id="@+id/list_recycler_view"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_weight="1" />

    </LinearLayout>
</layout>
```

우선 recycler view에 들어갈 아이템을 구성하는 레이아웃을 만듭니다.

list_recycler_view 라는 id를 가진 RecyclerView 위젯이 아래에 구현될 중첩 recycler view입니다.

## 하위 xml

```xml
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">

    <data>
    </data>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        android:background="@drawable/border"
        >
        <TextView
            android:id="@+id/product_name"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:gravity="center"
            android:hint="@string/product"/>
    </LinearLayout>
</layout>
```

중첩되어 들어갈 recycler view의 레이아웃입니다.

이 역시 위 레이아웃 코드와 마찬가지로 데이터 바인딩을 하기 위한 레이아웃 태그로 감싸져 있지만, 일반적인 레이아웃으로 해도 무관합니다.

하나의 레이아웃 형식으로만 유지하면 됩니다.

## 하위 recycler view adapter

```kotlin
class ListSubAdapter(private val context: Context, private val List: ArrayList<Count>) :
        RecyclerView.Adapter<ListSubAdapter.Holder>() {
    override fun onCreateViewHolder(p0: ViewGroup, p1: Int): Holder {
        val view = LayoutInflater.from(context).inflate(R.layout.sub_listview, p0, false)
        return Holder(view)
    }

    override fun getItemCount(): Int {
        return List.size
    }

    override fun onBindViewHolder(p0: Holder, p1: Int) {
        p0.bind(List[p1])
    }


    inner class Holder(itemView: View?) : RecyclerView.ViewHolder(itemView!!) {
        private val p = itemView?.findViewById<TextView>(R.id.product_name)

        fun bind(count: Count) {
            p?.text = count.productName
        }
    }
}
```

중첩이 되어있는 recycler view의 xml 파일과 상위 recycler view의 holder를 연결할 adapter를 만들어 줍니다.

inner class로 holder 클래스를 만들어서 활용합니다.

이 클래스는 단순히 레이아웃을 맵핑해주는 수준에 그칩니다.

## 상위 recycler view adapter

```kotlin
class ListAdapter(private val context: Context, private val List: ArrayList<List>) :
        RecyclerView.Adapter<ListAdapter.Holder>() {
    override fun onBindViewHolder(p0: Holder, p1: Int) {
        p0.bind(List[p1])
    }

    override fun onCreateViewHolder(p0: ViewGroup, p1: Int): Holder {
        val view = LayoutInflater.from(context).inflate(R.layout.listview, p0, false)
        return Holder(view)
    }

    override fun getItemCount(): Int {
        return List.size
    }

    inner class Holder(itemView: View?) : RecyclerView.ViewHolder(itemView!!) {
        private val s = itemView?.findViewById<TextView>(R.id.list_text)

        fun bind(list: List) {
            s?.text = list.name
            val s = itemView.findViewById<RecyclerView>(R.id.list_recycler_view)
            val mAdapter = ListSubAdapter(context,list.list_count)
            s.adapter = mAdapter
            val layoutmanager = LinearLayoutManager(context)
            s.layoutManager = layoutmanager
            s.setHasFixedSize(true)
        }
    }
}
```

이제는 하위 recycler view를 감싸고 있는 recycler view의 adapter를 만들어 보겠습니다.

아까 감싸진 recycler view의 adapter를 만드는 것과 크게 다를 것은 없지만, bind 함수가 조금 긴 것을 확인 할 수 있습니다.

그 이유는 감싸진 recycler view의 adapter를 가져와서 이 Holder 클래스에서 연결해주기 때문입니다.

일반적인 adapter 사용법과 별반 다를 것이 없지만, 차이점이 있다면 context를 가져오는 방식이 조금 다릅니다.

기존에서는 엑티비티를 가져왔다면, 이번에는 인자에 받아 온 context를 쓰기만 하면 됩니다.

## fragment xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">
    <data>
    </data>


    <android.support.constraint.ConstraintLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:layout_editor_absoluteY="81dp">

        <android.support.v7.widget.RecyclerView
            android:id="@+id/recyclerView"
            android:layout_width="0dp"
            android:layout_height="0dp"
            android:layout_marginBottom="8dp"
            android:layout_marginEnd="8dp"
            android:layout_marginStart="8dp"
            android:layout_marginTop="8dp"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toTopOf="parent" />
    </android.support.constraint.ConstraintLayout>
</layout>
```

fragment 레이아웃 xml 파일에서는 상위에 존재하는 하나의 RecyclerView 위젯만 배치하면 끝납니다.

## fragment

```kotlin
override fun onActivityCreated(savedInstanceState: Bundle?) {
    super.onActivityCreated(savedInstanceState)
    updateData() // 데이터를 생성해주는 메소드로서 해당 포스트에서는 설명이 생략된 메소드입니다.
    ListRecyclerView()
}
private fun ListRecyclerView() {
    val mAdapter = ListAdapter(activity!!, list)
    bind.recyclerView.adapter = mAdapter
    val layoutmanager = LinearLayoutManager(activity!!)
    bind.recyclerView.layoutManager = layoutmanager
    bind.recyclerView.setHasFixedSize(true)
}
```

fragment 클래스에서 중요한 메소드는 두개가 있습니다.

엑티비티가 만들어진 뒤에 실행되는 메소드인 onActivityCreated를 오버라이드하여 이 곳에서 adapter를 연결해줍니다.

