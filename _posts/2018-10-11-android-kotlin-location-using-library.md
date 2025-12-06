---
layout: post
title: 코틀린 프로젝트로 안드로이드에서 smartlocation 사용해보기
---

오늘은 코틀린 안드로이드로 사용자의 위치를 경도와 위도로 나타내주는 라이브러리를 사용해보려 합니다.

smartlocation라고 불리는 라이브러리를 사용하면 간단히 사용자의 기기에 있는 gps로 위치정보를 받아올 수 있습니다.

## 의존성 

```java
    implementation 'io.nlopez.smartlocation:library:3.3.3'
    implementation "com.google.android.gms:play-services-location:16.0.0"
```

app.gradle 파일에 위와 같이 smartlocation 라이브러리와 위치 관련 구글 플레이 서비스를 같이 넣어줍니다.

## 권한

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

manifest에 해당 권한을 추가해줍니다.

만약 안드로이드 6.0이상이라면 따로 권한을 요청하는 작업이 필요합니다.

해당 작업은 이전에 작성한 [권한에 대한 포스팅](https://minwook-shin.github.io/android-kotlin-request-permissions/)을 참고하시면 됩니다.

## GPS 시작

```kotlin
override fun onResume() {
    val provider: LocationGooglePlayServicesProvider? = LocationGooglePlayServicesProvider()
    provider?.setCheckLocationSettings(true)
    val smartLocation = SmartLocation.Builder(this).logging(true).build()
    smartLocation.location(provider).start(this)
    smartLocation.activity().start(this)
    val lastLocation = SmartLocation.with(this).location().lastLocation
    if (lastLocation != null) {
        toast(lastLocation.latitude.toString() + " " + lastLocation.longitude.toString())
    }
}
```

LocationGooglePlayServicesProvider 타입의 provider 객체를 만들고, 이 객체에서 설정을 확인하도록 참으로 변경합니다.

smartLocation라고 객체를 만들고 위치 서비스를 켜줍니다.

마지막으로 해당 엑티비티의 생명주기가 Resume 상태일 때에 이 위치 서비스에서 내려오는 경도와 위도를 토스트로 출력합니다.

## GPS 중단

```kotlin
override fun onStop() {
    super.onStop()
    SmartLocation.with(this).location().stop()
    SmartLocation.with(this).activity().stop()
    SmartLocation.with(this).geofencing().stop()
}
```

앱이 꺼지기 전에 이전에 켜둔 서비스들을 멈춰주어야 앱을 빌드했을 때에 오류가 발생하지 않습니다.