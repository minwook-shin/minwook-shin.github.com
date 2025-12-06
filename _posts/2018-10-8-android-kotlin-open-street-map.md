---
layout: post
title: 코틀린 프로젝트로 안드로이드에서 OpenStreetMap 쉽게 사용해보기
---

오늘은 코틀린 안드로이드 프로젝트에서 오픈 스트리트 맵을 쉽게 사용해보려 합니다.

최근 구글맵의 무료 사용 범위가 축소됨에 따라 오픈 스트리트 맵이 대안으로 떠오르고 있습니다.

## 의존성

```java
    implementation "org.osmdroid:osmdroid-android:6.0.2"
```

## 권한

```xml
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"  />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

위치 권한, 인터넷 권한, 맵을 저장항 저장소 권한이 필요합니다.

안드로이드 6.0 이상부터는 따로 권한을 요청하는 작업이 필요합니다.

해당 작업은 이전에 작성한 [권한에 대한 포스팅](https://minwook-shin.github.io/android-kotlin-request-permissions/)을 참고하시면 됩니다.

## 레이아웃 

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:orientation="vertical">

    <org.osmdroid.views.MapView
        android:id="@+id/map"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</LinearLayout>
```

맵을 레이아웃의 전체를 덮는 예제입니다.

id는 map이라고 부여했습니다.

## 클래스

레이아웃을 만들었으면 이제 클래스에서 사용해보겠습니다.

```kotlin
Configuration.getInstance().userAgentValue = BuildConfig.APPLICATION_ID
map.setTileSource(TileSourceFactory.MAPNIK)
```

우선 앱의 id를 타일 서버에 보내어 서버에서 인증을 거칩니다.

```kotlin
map.setBuiltInZoomControls(true)
map.setMultiTouchControls(true)
map.isClickable = true
map.controller.setZoom(19.0)
val startPoint = GeoPoint(37.6096409, 126.99769700000002)
map.controller.setCenter(startPoint)
```

맵의 줌 컨트롤러와 멀티터치를 지원하게 설정합니다.

맵의 컨트롤러로 맵의 줌을 미리 설정할 수 있습니다.

그리고 맵의 시작점을 설정할 수 있습니다.

```kotlin
val locationOverlay = ItemizedIconOverlay(addPoint(), object : ItemizedIconOverlay.OnItemGestureListener<OverlayItem> {
    override fun onItemSingleTapUp(i: Int, overlayItem: OverlayItem): Boolean {
        longToast(overlayItem.title + " " + overlayItem.snippet)
        return true
    }
    override fun onItemLongPress(i: Int, overlayItem: OverlayItem): Boolean {
        return false
    }
}, activity!!.applicationContext)
```

맵의 포인트에 클릭했을 때에 수행할 작업을 이벤트 리스너로 등록해줍니다.

위 예쩨는 addPoint 함수에서 설정한 포인트에 클릭했을 때에 토스트로 미리 설정한 제목과 설명이 출력됩니다. 

```kotlin
this.map.overlays.add(locationOverlay)
```

위에서 생성한 이벤트 리스너를 맵에 등록해줍니다.

```kotlin
private fun addPoint(): ArrayList<OverlayItem> {
        val overlayItemArrayList : ArrayList<OverlayItem> = ArrayList()
        val geoPoint = GeoPoint(37.6096409, 126.99769700000002)
        val overlayItem = OverlayItem("kookmin.univ", "snippet", geoPoint)
        val markerDrawable = ContextCompat.getDrawable(activity!!.applicationContext, R.drawable.ic_launcher_foreground)
        overlayItem.setMarker(markerDrawable)
        overlayItemArrayList.add(overlayItem)
        return overlayItemArrayList
    }
```

방금 설명한 addPoint 함수를 구형해야 합니다.
반환 형식은 OverlayItem 타입을 가진 요소가 들어있는 ArrayList 입니다.

GeoPoint 타입의 위치 정보를 저장할 객체를 만들어주고, 각각의 아이템을 만들면서 넣어줍니다.
마커에 표시할 이미지를 getDrawable로 가져옵니다. 이 때에 프래그먼트에서 가져오려면 activity의 context를 가져옵니다.

OverlayItem 타입을 가진 요소가 들어있는 ArrayList에 각각의 OverlayItem 객체를 넣어줍니다.


이제 앱 빌드를 시작하면 원하는 위치에서 맵이 불러오게 되며, 원하는 위치에 핀이 찍히게 됩니다.