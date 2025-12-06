---
layout: post
title: 코틀린으로 작성한 안드로이드 drawer layout 사용하기
---

오늘은 코틀린으로 안드로이드에서 drawer layout을 사용하여 엑티비티에 코드를 작성해보려 합니다.

## 레이아웃

```xml
<android.support.v4.widget.DrawerLayout
    android:id="@+id/drawer"
    android:layout_width="0dp"
    android:layout_height="0dp"
    android:layout_marginBottom="8dp"
    android:layout_marginStart="8dp"
    app:layout_constraintBottom_toBottomOf="parent"
    app:layout_constraintLeft_toLeftOf="parent"
    app:layout_constraintRight_toRightOf="parent"
    app:layout_constraintTop_toTopOf="parent">

        <android.support.constraint.ConstraintLayout
            android:id="@+id/constraintLayout"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginBottom="3dp">
        </android.support.constraint.ConstraintLayout>

        <android.support.constraint.ConstraintLayout
            android:layout_width="400dp"
            android:layout_height="match_parent"
            android:layout_gravity="end"
            android:background="#009688"
            android:gravity="center">
        </android.support.constraint.ConstraintLayout>

</android.support.v4.widget.DrawerLayout>
```

DrawerLayout이 덮는 화면과 옆에서 나올 DrawerLayout 화면을 감싸줍니다.

DrawerLayout이 덮는 화면은 레이아웃의 화면을 wrap_content로 맞춰주고, 나올 화면은 높이만 match_parent로 맞추고 넓이는 덮는 화면의 크기를 결정하므로 원하는 대로 설정해줍니다.

위 예시는 ConstraintLayout을 사용했지만, 다른 레이아웃을 사용해도 됩니다.

만약 터치로 인해 drawer layout이 덮히는 화면의 이벤트가 작동하게되면 나오는 drawer 부분을 

```xml
android:clickable="true"
android:focusable="true"
```

위 옵션들을 추가해주기 바랍니다.

## 이벤트

```java
bind.drawer.openDrawer(GravityCompat.END)
```

drawer layout을 열게 해줍니다.

```java
bind.drawer.closeDrawer(GravityCompat.END)
```

drawer layout을 닫게 해줍니다.

```java
bind.drawer.isDrawerOpen(GravityCompat.END)
```

drawer layout가 열려있는지 boolean 값으로 나타내줍니다.