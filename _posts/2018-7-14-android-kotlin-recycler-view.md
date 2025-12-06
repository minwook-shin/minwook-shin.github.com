---
layout: post
title: 코틀린으로 작성한 안드로이드 Recycler view 에 대하여 알아보기
---

오늘은 안드로이드에서 코틀린으로 Recycler view를 사용하는 방식을 서술하려 합니다.

## 설정

Recycler view는 다른 위젯과 다르게 기본적으로 포함되어있지 않기 때문에 추가해주어야 합니다.

앱수준의 biuld.gradle 파일에 Recycler view 라이브러리를 추가해줍니다.

```java
implementation 'com.android.support:recyclerview-v7:28.0.0-alpha3'
```

## 객체 생성 

```java
class Product(
        var Name: String?,
        var quantity: Int
)
```

한 세트의 뷰에서 보일 데이터들을 한 클래스로 묶습니다.

나중에 객체로 생성되어 값을 저장하게 됩니다.

## 레이아웃 생성

```java
<?xml version="1.0" encoding="utf-8"?>
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
            android:hint="@string/product"
            android:text=""
            android:textAppearance="@style/TextAppearance.AppCompat.Caption" />

        <TextView
            android:id="@+id/quantity_number"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:gravity="right"
            android:hint="@string/quantity"
            android:text=""
            android:textAppearance="@style/TextAppearance.AppCompat.Caption" />
    </LinearLayout>
</layout>
```

한 라인을 구현해주는 레이아웃입니다.

방금 만든 한 데이터 세트가 이 레이아웃에 들어가게 됩니다.

본 xml 코드는 layout로 감싸져있는 형태인 데이터 바인딩을 하기 위한 코드로 변경되어 있어서, 데이터 바인딩을 사용하지 않는 분은 다르게 코드를 작성해도 무방합니다.

## 어뎁터 생성

Recycler view는 Adapter를 만들어서 데이터를 넣어주어야 합니다.

기본적으로 안드로이드에서 RecyclerView.Adapter를 제공해주므로 이를 상속받아서 구현하면 편합니다.

```kotlin
class CountRecyclerViewAdapter(private val context: Context, private val List: ArrayList<Count>) :
        RecyclerView.Adapter<CountRecyclerViewAdapter.Holder>() {
    override fun onCreateViewHolder(p0: ViewGroup, p1: Int): Holder {
        val view = LayoutInflater.from(context).inflate(R.layout.아이템-레이아웃)))), p0, false)
        return Holder(view)
    }

    override fun getItemCount(): Int {
        return List.size    
    }

    override fun onBindViewHolder(p0: Holder, p1: Int) {
        p0.bind(List[p1])
    }


    inner class Holder(itemView: View?) : RecyclerView.ViewHolder(itemView!!) {
        private val q = itemView?.findViewById<TextView>(R.id.quantity_number)
        private val p = itemView?.findViewById<TextView>(R.id.product_name)

        fun bind(count: Count) {
            q?.text = count.quantity.toString()
            p?.text = count.productName
        }
    }
}
```

RecyclerView.Adapter<>()를 상속받아서 오버라이드해주어야 할 메소드들을 구현해줍니다.

뷰 홀더를 inner class로 만들어서 방금만든 아이템 레이아웃에 존재하는 텍스트 뷰에 바인드해줍니다.


## 엑티비티 작성

```kotlin
val mAdapter = CountRecyclerViewAdapter(this@CountActivity, countList)
```

방금 만든 아이템 클래스의 형식인 리스트를 인자로 담은 Adapter 클래스의 객체를 생성해줍니다. 

```kotlin
mRecyclerView.adapter = mAdapter
```

레아이웃에 Recycler view를 배치한 id의 어뎁터에 해당 Adapter 클래스의 객체를 할당해줍니다.

```kotlin
val layoutManager = LinearLayoutManager(this@CountActivity)
mRecyclerView.layoutManager = layoutManager
```

LayoutManager를 설정해줍니다.

```kotlin
mRecyclerView.setHasFixedSize(true)
```

각 아이템들이 Recycler view 전체 크기를 변경할 예정이므로 true로 설정합니다.

```kotlin
countList.add(data)
adapter.notifyDataSetChanged()
```

리스트에 원하는 정보를 넣고 notifyDataSetChanged() 메서드를 불러옵니다.