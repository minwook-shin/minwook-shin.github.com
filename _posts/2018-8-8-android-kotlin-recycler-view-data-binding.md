---
layout: post
title: 코틀린으로 작성한 안드로이드 Recycler view adpater에 데이터 바인딩 접목하기
---

오늘은 안드로이드에서 코틀린으로 Recycler view를 데이터 바인딩으로 사용하는 방식을 서술하려 합니다.

## adapter

```kotlin
class RecyclerViewAdapter(context : Context, dataSet:ArrayList<String>) : RecyclerView.Adapter<RecyclerView.ViewHolder>() {
    lateinit var itemBinding:ViewListItemBinding

    override fun onCreateViewHolder(p0: ViewGroup, p1: Int): RecyclerView.ViewHolder {
        val layoutInflater = LayoutInflater.from(p0.context)
        itemBinding = DataBindingUtil.inflate(layoutInflater,R.layout.view_list_item, p0, false)
        return ViewHolder(itemBinding.root.rootView)
    }
```

onCreateViewHolder 메소드를 오버라이드해줍니다.

RecyclerView의 ViewHolder 클래스 타입을 반환하는 메소드이며, DataBindingUtil으로 데이터 바인딩을 구현할 수 있습니다.

```kotlin
    override fun onBindViewHolder(p0: RecyclerView.ViewHolder, p1: Int) {
        val holder: ViewHolder = p0 as ViewHolder
        if (0 <= p1 && p1 < dataSet.size) {
            val data = dataSet[p1]
            holder.bind(data)
        }
    }
    override fun getItemCount(): Int {
        return dataSet.size
    }
```

onBindViewHolder 메소드로 데이터와 ViewHolder를 엮어주고, getItemCount 메소드로 데이터의 갯수를 반환해줍니다.

```kotlin
    private inner class ViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView) {
        fun bind(data: String) {
            itemBinding.enterText.text= data
        }
    }
}
```

inner class로 ViewHolder를 만들어주어 뷰와 데이터를 맵핑해줍니다. 

## activity

```kotlin
val userArray = ArrayList<String>()
array.add("1")
array.add("2")
array.add("3")

recyclerView(userArray)

private fun recyclerView(userArray: ArrayList<String>) {
    bind.mRecyclerView.layoutManager = LinearLayoutManager(activity)
    adapter = RecyclerViewAdapter(activity!!.applicationContext, array)
    bind.mRecyclerView.adapter = adapter
    bind.mRecyclerView.setHasFixedSize(true)
}
```

평소와 같이 layoutManager도 설정하고, 구현한 RecyclerViewAdapter도 객체로 만들어서 RecyclerView에 넣어줍니다.

activity!!.applicationContext는 이 코드가 사용된 곳이 fragment라서 쓰인 코드이므로, 일반적인 activity라면 바로 context를 불러오면 됩니다.

## binding adapter

```xml
<data>
    <import type="android.view.View"/>
    <variable
        name="text"
        type="android.databinding.ObservableField&lt;String&gt;"/>
</data>
```

이 글에 자세히 설명되진 않았지만, recycler view에도 바인딩 어댑터를 사용할 수 있습니다.