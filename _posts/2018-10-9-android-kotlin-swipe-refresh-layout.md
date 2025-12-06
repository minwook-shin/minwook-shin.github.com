---
layout: post
title: 코틀린 프로젝트로 안드로이드에서 SwipeRefreshLayout 쉽게 사용해보기
---

오늘은 코틀린 안드로이드 프로젝트에서 SwipeRefreshLayout을 쉽게 사용해보려 합니다.

SwipeRefreshLayout을 recycler view에서 하단으로 끌어 당겨서 새로고침할 때 쓸 수 있습니다.

## 의존성

```java
    implementation 'androidx.appcompat:appcompat:1.0.0'
```

원래 android.support 라이브러리에 있지만, 이제 androidx 패키지로 통합되었기 때문에 위와 같이 작성해줍니다.

## 레이아웃

```xml
<?xml version="1.0" encoding="utf-8"?>

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#ffffff"
    tools:layout_editor_absoluteY="81dp">

    <androidx.swiperefreshlayout.widget.SwipeRefreshLayout
        android:id="@+id/swipe"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
        <androidx.recyclerview.widget.RecyclerView
            android:id="@+id/list"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:layout_margin="2dp" />

    </androidx.swiperefreshlayout.widget.SwipeRefreshLayout>

</LinearLayout>
```

보통 위와 같이 RecyclerView를 감싸는 형테로 사용됩니다.

## 클래스

이번 예제에서는 엑티비티가 아닌 프래그먼트로 작성했지만, 엑티비티도 다를 것은 별로 없습니다.

```kotlin
class RealFragment : BaseFragment(), SwipeRefreshLayout.OnRefreshListener{
    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        return inflater.inflate(R.layout.fragment_friends,container,false)
    }

    override fun onRefresh() {
        // 새로고침
    }

    override fun onActivityCreated(savedInstanceState: Bundle?) {
        super.onActivityCreated(savedInstanceState)
        swipe.setOnRefreshListener(this)
    }
}
```

레이아웃에서 SwipeRefreshLayout의 id를 가져와 onRefresh 인터페이스를 등록해줍니다.

onRefresh() 메소드를 오버라이딩하여 아래로 당겼을 때에 원하는 동작을 수행하도록 합니다.

이제 앱을 빌드하면 원하는 recycler view에서 당겨서 새로고침을 할 수 있게 됩니다.