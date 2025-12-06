---
layout: post
title: 코틀린으로 안드로이드에서 BottomNavigationView와 fragment 연결하기
---

오늘은 안드로이드에서 FrameLayout에다가 fragment를 넣어서 BottomNavigationView로 컨트롤하여 동작하는 것을 코틀린으로 작성해보려 합니다.

또한 이 포스트는 data binding을 적용했으므로 layout 태그와 bind 객체가 포함되어 있습니다. 양해부탁드립니다.

## 설정

BottomNavigationView를 이용하기 위해서는 design 라이브러리가 필요합니다.

```java
implementation 'com.android.support:design:28.0.0-beta01'
```

## 레이아웃

fragment 레이아웃을 준비합니다.

아래는 하나의 fragment만 준비한 코드이므로 여러 fragment를 준비하려면 여러개의 파일을 생성해야 합니다.

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

이 fragment를 넣을 메인 엑티비티의 레이아웃도 준비합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">

    <data>
    </data>

    <android.support.constraint.ConstraintLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".View.MainActivity">

        <FrameLayout
            android:id="@+id/fragment_container"
            android:layout_width="0dp"
            android:layout_height="0dp"
            app:layout_constraintTop_toBottomOf="parent"
            app:layout_constraintBottom_toTopOf="@+id/navigation_view"
            app:layout_constraintLeft_toLeftOf="parent"
            app:layout_constraintRight_toRightOf="parent"
            android:background="#f2f2f2"
            android:elevation="1dp"/>

        <android.support.design.widget.BottomNavigationView
            android:id="@+id/navigation_view"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintLeft_toLeftOf="parent"
            app:layout_constraintRight_toRightOf="parent"
            app:menu="@menu/menu_navigation"
            />
    </android.support.constraint.ConstraintLayout>
</layout>
```

이 엑티비티 xml에는 design 라이브러리에 있는 BottomNavigationView도 추가합니다.

BottomNavigationView에 menu라는 속성으로 매뉴를 만들 수 있습니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">

    <item
        android:id="@+id/navigation_a"
        android:icon="@drawable/button"
        android:title="a" />

    <item
        android:id="@+id/navigation_b"
        android:icon="@drawable/button"
        android:title="b" />

    <item
        android:id="@+id/navigation_c"
        android:icon="@drawable/button"
        android:title="c" />

</menu>
```

매뉴에 넣을 아이콘과 타이틀 텍스트도 넣어주면 됩니다.

## fragment

```kotlin
class FragmentA : BaseFragment(){
    lateinit var bind: FragmentABinding
    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        bind = DataBindingUtil.inflate(inflater, R.layout.fragment_a, container, false)
        return bind.root
    }
    override fun onActivityCreated(savedInstanceState: Bundle?) {
        super.onActivityCreated(savedInstanceState)
    }
}
```

fragment 클래스를 준비합니다.

xml과 마찬가지로 하나의 fragment만 준비한 코드이므로 여러 fragment를 준비하려면 여러개의 파일을 생성해야 합니다.

onActivityCreated 메소드에다가 사용할 메소드를 넣어주면 됩니다.

## activity

```kotlin
class MainActivity : BaseActivity(), BottomNavigationView.OnNavigationItemSelectedListener
```

엑티비티는 기존과 다르게 BottomNavigationView.OnNavigationItemSelectedListener를 상속받아서 
onNavigationItemSelected 메소드를 오버라이드합니다.

```kotlin
val bottomNavigationView = findViewById<View>(R.id.navigation_view) as BottomNavigationView
bottomNavigationView.setOnNavigationItemSelectedListener(this)
```

엑티비티에서 NavigationItemSelectedListener를 오버라이드한 메소드에 연결해줍니다.

```kotlin
override fun onNavigationItemSelected(p0: MenuItem): Boolean {
        when(p0.itemId){
            R.id.navigation_a ->{ 
                val fragmentA = FragmentA()
                supportFragmentManager.beginTransaction().replace(R.id.fragment_container,fragmentA).commit()
            }
            R.id.navigation_b -> {
                val fragmentB = FragmentB()
                supportFragmentManager.beginTransaction().replace(R.id.fragment_container,fragmentB).commit()
            }
            R.id.navigation_c -> {
                val fragmentC = FragmentC()
                supportFragmentManager.beginTransaction().replace(R.id.fragment_container,fragmentC).commit()
            }
        }
        return true
}
```

onNavigationItemSelected 메소드에는 인자로 어떤 매뉴가 클릭되는지 들어오게 됩니다.

fragment 클래스의 객체를 만들어주며 만들어준 객체를 supportFragmentManager의 beginTransaction()에 았는 replace 메소드에다가 FrameLayout의 id와 미리 만들어둔 fragment 객체를 넣어줍니다.

이와 같이 코드를 작성하고 빌드를 수행하면 정상적으로 구동됨을 확인할 수 있습니다.