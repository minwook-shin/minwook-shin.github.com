---
layout: post
title: 코틀린 프로젝트로 안드로이드에서 OpenStreetMap 눌러서 이벤트 사용하기
---

오늘은 코틀린 안드로이드 프로젝트에서 오픈 스트리트 맵을 눌렀을 때 발생되는 이벤트를 사용해보려 합니다.

기존에 마커를 눌러 사용하던 방식은 이미 글이 올라와 있지만, 마커를 생성하는 유무에 약간 차이가 있어서 포스팅합니다.

해당 포스트에서는 마커를 생성하지 않고, 지도를 짧게 누르거나 길게 누를 때에 발생할 수 있는 이벤트를 만드려고 합니다.

## 의존성

```java
    implementation "org.osmdroid:osmdroid-android:6.0.2"
```

기존에 오픈스트리트 맵을 사용하기 위한 것처험 osmdroid 의존성을 추가합니다.

## 권한

```xml
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"  />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

기존에 지도를 사용했던 것처럼 위치 권한, 인터넷 권한, 지도 데이터를 저장할 저장소 권한이 필요합니다.

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

맵을 레이아웃의 전체를 덮는 예제는 위와 같습니다.

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

오픈스트리트 맵의 레이아웃 id로 줌 컨트롤러와 멀티터치를 지원하게 설정합니다.

지도의 컨트롤러로 지도의 확대 배율을 미리 설정할 수 있습니다.

그리고 지도의 시작점을 설정할 수 있습니다.

```kotlin
val receiver = object :MapEventsReceiver{
    override fun longPressHelper(p: GeoPoint?): Boolean {
        map.snackbar("길게 눌렀습니다.")
        return true
    }
    override fun singleTapConfirmedHelper(p: GeoPoint?): Boolean {
        map.snackbar("짧게 눌렀습니다.")
        return false
    }
}
```

지도를 클릭했을 때에 수행할 작업을 이벤트 리스너로 등록해줍니다.

longPressHelper와 singleTapConfirmedHelper 메소드가 오버라이딩되며 각각 길게 누르거나 짧게 누를 때에 수행되게 됩니다.

```kotlin
Log.d(tag,p?.latitude)
Log.d(tag,p?.longitude)
```

참고로 각각 오버라이드된 메소드에서 제공하는 GeoPoint 인자에서 위도와 경도를 받을 수 있습니다.

```kotlin
val overlayEvents = MapEventsOverlay(baseContext, receiver)
this.map.overlays.add(locationOverlay)
```

오버라이딩하여 메소드를 구현했다면 이제 위에서 생성한 이벤트 리스너를 맵에 등록해줍니다.


이제 앱 빌드를 시작하면 지도가 출력되고, 출력된 지도에 터치를 하면 위 이벤트가 수행되는 것을 확인할 수 있습니다.