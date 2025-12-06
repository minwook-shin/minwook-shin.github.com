---
layout: post
title: 코틀린으로 작성한 안드로이드에서 핸드폰 번호 가져오기
---

오늘은 안드로이드 앱을 설치한 기기의 핸드폰 번호를 코틀린으로 구해보려 합니다.

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

위 권한 얻기는 이전 포스트인 uuid 얻어오기와 방식이 똑같습니다. 참고하시기 바랍니다.

## 핸드폰 번호 가져오기

```kotlin
private fun getPhoneNumber() {
        val msg =App.INSTANCE.context().applicationContext.getSystemService(Context.TELEPHONY_SERVICE) as TelephonyManager
        if (msg.line1Number != null) {
            val phoneNum = msg.line1Number.toString()
            App.INSTANCE.myUserInfo.userPhone = phoneNum
        }
        else{
            App.INSTANCE.myUserInfo.userPhone = "Unknown"
        }
}
```

우선 Application 클래스를 상속받은 App 클래스에서 context를 받아와서(만약 activity에서 바로 사용한다면 생략해도 무관합니다.) getSystemService 메소드로 TelephonyManager 타입의 변수에 값을 넣습니다.

usim이 없는 핸드폰인 경우에는 msg.line1Number가 null이므로 예외를 처리해주도록 합니다.

만약 핸드폰에 유심이 있다면 myUserInfo라는 글로벌 객체의 userPhone 속성으로 값을 넣었습니다.
아니면, 임의의 값을 넣어주고 메소드를 마무리짓습니다.
