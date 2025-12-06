---
layout: post
title: 코틀린으로 작성한 안드로이드 오레오 이상 버전에서 필요한 권한 요청 사용하기
---

안드로이드 6.0 오레오부터 권한을 요청해야 되는 방식이 존재합니다.

물론 모든 권한이 그러한 것이 아니고, 일부 위험하다고 판단되는 권한들만 그럽니다.

오늘은 안드로이드 6.0 오레오부터 달라진 권한 획득 방식을 코틀린으로 알아보려 합니다.

## 기존 권한 획득 

```xml
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.CAMERA" />
```

## 권한 확인

```java
if (ContextCompat.checkSelfPermission(thisActivity, Manifest.permission.READ_PHONE_STATE) != PackageManager.PERMISSION_GRANTED) {

}
```

checkSelfPermission 메소드로 권한을 얻었는지 확인할 수 있습니다.

## 함수 만들기

```java
ActivityCompat.requestPermissions(this, arrayOf(Manifest.permission.WRITE_EXTERNAL_STORAGE, Manifest.permission.CAMERA))
```

ActivityCompat의 requestPermissions 메소드로 리스트로 묶인 권한을 요청할 수 있습니다

## 오버라이드

```kotlin
override fun onRequestPermissionsResult(requestCode: Int, permissions: Array<String>, grantResults: IntArray) {
    when (requestCode) {
        Constants.RECORD_REQUEST_CODE -> {
            if (grantResults[0] != PackageManager.PERMISSION_GRANTED || grantResults[1] != PackageManager.PERMISSION_GRANTED || grantResults[2] != PackageManager.PERMISSION_GRANTED) {
            } else {
                // pass event
            }
        }
    }
}
```

OnRequestPermissionResult는 권한 요청의 결과를 받는 오버라이드된 메소드입니다.
