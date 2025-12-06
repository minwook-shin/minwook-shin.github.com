---
layout: post
title: 코틀린으로 작성한 안드로이드에서 UUID 가져오기
---

오늘은 범용 고유 식별자라고 불리는 UUID를 안드로이드에서 코틀린으로 구해보려 합니다.

## 설정

```xml
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
```

안드로이드 오레오 6.0 이하 버전에서는 위와 같이 간단하게 처리할 수 있습니다.

```kotlin
private fun makeRequest() {
        if (ContextCompat.checkSelfPermission(this@MainActivity, Manifest.permission.READ_PHONE_STATE) != PackageManager.PERMISSION_GRANTED) {
            ActivityCompat.requestPermissions(this@MainActivity,
                    arrayOf(Manifest.permission.READ_PHONE_STATE),
                    Constants.RECORD_REQUEST_CODE)
        }

}

override fun onRequestPermissionsResult(requestCode: Int,
                                            permissions: Array<String>, grantResults: IntArray) {
    when (requestCode) {
        Constants.RECORD_REQUEST_CODE -> {
            if (grantResults[0] != PackageManager.PERMISSION_GRANTED || grantResults[1] != PackageManager.PERMISSION_GRANTED || grantResults[2] !=PackageManager.PERMISSION_GRANTED) {
                // 권한을 못 받을 경우
            } else {
                //권한을 받을 경우
            }
        }
    }
}
```

checkSelfPermission 메소드로 권한을 얻었는지 확인한 뒤에 못 얻었다면 requestPermissions 메소드로 권한을 받아옵니다.

onRequestPermissionsResult 메소드를 오버라이드하여 권한을 받거나 못 받을 때에 수행할 명령을 적어줍니다.

## uuid 가져오기


```kotlin
private fun getUuid() : String {
    val uuid =  android.provider.Settings.Secure.getString(Context().contentResolver,Settings.Secure.ANDROID_ID)
    return uuid
}
```

위와 같이 작성하면 해당 안드로이드 기기의 uuid가 반환됩니다.