---
layout: post
title: 코틀린으로 작성한 안드로이드에서 fragment를 이용해 viewpager 사용하기
---

오늘은 안드로이드에서 여러 fragment를 이용하여 viewpager를 사용해보는 것을 코틀린을 통하여 합니다.

## 설정

```java
implementation 'com.android.support:design:28.0.0-alpha3'
```

디자인 지원 라이브러리를 추가해주고 gradle sync를 합니다.

## fragment

각각의 페이지를 담당하는 fragment를 만들 차례입니다.

만들어진 fragment들은 viewpager에 담길 예정입니다.

우선 fragment 코틀린 파일을 만들어줍니다.

```kotlin
class ProductReportFragment : BaseFragment(){
    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {

        return inflater.inflate(R.layout.fragment, container, false)
    }
}
```

BaseFragment는 Fragment를 상속받는다고 가정합니다.

onCreateView 메소드를 오버라이드하여 작성합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">
    <data>
    </data>

    <ScrollView
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:fillViewport="true">

        <android.support.constraint.ConstraintLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            tools:layout_editor_absoluteY="81dp">

            <TextView
                android:id="@+id/test_text"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_centerInParent="true"
                android:text="first tab"
                android:textAppearance="?android:attr/textAppearanceLarge"
                app:layout_constraintStart_toStartOf="parent"
                app:layout_constraintTop_toTopOf="parent" />

        </android.support.constraint.ConstraintLayout>
    </ScrollView>
</layout>
```

한 fragment에 나타낼 xml 파일을 만듭니다.

데이터 바인딩을 위한 형식이므로 ConstraintLayout만 작성해도 무관합니다.

그 다음에는 본인이 원하는 fragment의 갯수대로 계속 복사하며 작성해줍니다.

## adapter

```kotlin
class TabAdapter(fm: FragmentManager, private var tabCount: Int) : FragmentStatePagerAdapter(fm){
    override fun getItem(p0: Int): Fragment? {
        when (p0) {
            0 -> return FirstFragment()
            1 -> return SecondFragment()
        }
        return null
    }

    override fun getCount(): Int {
        return tabCount
    }
}
```

viewpager와 tab layout을 이어줄 adapter를 만들어줍니다.

FragmentStatePagerAdapter를 상속받아서 FragmentManager 형식의 fm 변수를 담은 상위 생성자를 호출합니다.

자바에서는 super(fm)으로 나타내는 코드입니다.

그리고 getItem 메소드와 getCount 메소드를 오버라이드하여 구현해줍니다.

getItem는 인자에 인덱스 값이 들어와서 각각의 fragment 클레스들을 맵핑해주게 해줍니다.

getCount는 해당 클래스에서 tabLayout.tabCount를 반환해줍니다.



## main

```kotlin
class Activity : BaseActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        bind
        
        bind.tabLayout.addTab(bind.tabLayout.newTab().setText(getString(R.string.first)))
        bind.tabLayout.addTab(bind.tabLayout.newTab().setText(getString(R.string.second)))

        val adapter = TabAdapter(supportFragmentManager, bind.tabLayout.tabCount)
        bind.pager.adapter = adapter

        bind.pager.addOnPageChangeListener(TabLayout.TabLayoutOnPageChangeListener(bind.tabLayout))

        bind.tabLayout.addOnTabSelectedListener(object : TabLayout.OnTabSelectedListener {
            override fun onTabReselected(p0: TabLayout.Tab?) {}
            override fun onTabUnselected(p0: TabLayout.Tab?) {}
            override fun onTabSelected(tab: TabLayout.Tab) {
                bind.pager.currentItem = tab.position
            }
        })
    }
}
```

우선 BaseActivity는 AppCompatActivity를 상속받았다고 가정합니다.

tab Layout에 있는 addTab 메소드로 탭을 추가해줍니다.

이전에 만든 TabAdapter 클래스 인자에 supportFragmentManager와 tab Layout의 tabCount를 넣어주고,  adaper객체를 만들어 viewpager와 연결합니다.

TabLayoutOnPageChangeListener를 만들어서 viewpager에 추가해줍니다.

OnTabSelectedListener로 탭이 선택되면 현재 보이는 viewpager를 선택한 탭의 인덱스로 바꾸어주게 해줍니다.

마지막으로 빌드를 해보면 만들었던 fragment가 viewpager에 순서대로 들어가 있음을 확인할 수 있으며, tab layout도 정상적으로 viewpager와 연결되었음을 확인할 수 있습니다.
