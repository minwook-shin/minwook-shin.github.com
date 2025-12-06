---
layout: post
title: 코틀린으로 작성한 안드로이드 바코드 인식하기에 대하여 알아보기
---

오늘은 안드로이드에서 바코드를 인식하는 라이브러리인 zxing을 코틀린으로 작성해보았습니다.

## 설정 

```java
implementation 'me.dm7.barcodescanner:zxing:1.9.8'
```

라이브러리를 가져옵니다.

## 카메라 권한

```java
<uses-permission android:name="android.permission.CAMERA" />
```

바코드 인식에는 카메라 권한이 필요하므로, 카메라 권한을 가져옵니다.

안드로이드 6.0 이상이면, 따로 권한을 물어보아야 하므로 아래와 같은 코드를 권한을 물어볼 엑티비티에 코드를 넣어줍니다.

```kotlin
ActivityCompat.requestPermissions(this,arrayOf(Manifest.permission.READ_PHONE_STATE, Manifest.permission.WRITE_EXTERNAL_STORAGE, Manifest.permission.CAMERA),Constants.RECORD_REQUEST_CODE)

override fun onRequestPermissionsResult(requestCode: Int,permissions: Array<String>, grantResults: IntArray) {
    when (requestCode) {
        Constants.RECORD_REQUEST_CODE -> {
            if (grantResults[0] != PackageManager.PERMISSION_GRANTED || grantResults[1] != PackageManager.PERMISSION_GRANTED || grantResults[2] != PackageManager.PERMISSION_GRANTED) {
            } else {
                Handler().postDelayed({
                    val intent = Intent(this@SplashActivity, MainActivity::class.java)
                    intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP)
                    startActivity(intent)
                }, 2500)
            }
        }
    }
}
```

onRequestPermissionsResult 메소드를 오버라이딩해서 권한이 다 들어오게 될 때만 다른 엑티비티로 넘어가게 작성했습니다.


## 엑티비티

이제 카메라 화면이 나오는 엑티비티를 만들어야 합니다.

```kotlin
class ScanActivity  : AppCompatActivity(), ZXingScannerView.ResultHandler{
    private var mScannerView: ZXingScannerView? = null

    public override fun onCreate(state: Bundle?) {
        super.onCreate(state)
        mScannerView = ZXingScannerView(this)
        setContentView(mScannerView)
    }

    public override fun onResume() {
        super.onResume()
        mScannerView!!.flash = true
        mScannerView!!.setAutoFocus(true)
        mScannerView!!.setResultHandler(this)
        mScannerView!!.startCamera()
    }

    public override fun onPause() {
        super.onPause()
        mScannerView!!.stopCamera()
    }

    override fun handleResult(rawResult: Result) {
        val intent = Intent(this@ScanActivity, InActivity::class.java)
        intent.putExtra("result_msg", rawResult.text)
        setResult(RESULT_OK, intent)
        finish()
    }

}
```

ZXingScannerView.ResultHandler를 상속받아서 onCreate, onResume, onPause, handleResult를 오버라이드해줍니다.

엑티비티가 시작되었을 때에 onCreate와 onResume이 실행되므로, 켜지자마자 mScannerView 객체가 생성되며 각종 옵션(플래시, 오토포커스)들을 boolean 값으로 설정할 수 있습니다.

그리고 startCamera 메소드로 카메라가 작동하게 됩니다.

onPause, 즉 중지되었을 때에는 stopCamera 메소드로 인해 카메라가 꺼지게 되고, 오버라이드한 handleResult 메소드로 인식된 바코드를 putExtra를 통해 전달하게 됩니다.


## 메인으로 값 가져오기

```kotlin
val intent = Intent(this@CountActivity, ScanActivity::class.java)
            startActivityForResult(intent, Constants.REQUEST1)
```

처음에 카메라 엑티비티로 넘어가기 전에 startActivityForResult 메소드로 값을 반환 받을 준비도 합니다.


```kotlin
var getStr = ""
override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        super.onActivityResult(requestCode, resultCode, data)
        if (requestCode == Constants.REQUEST1) {
            if (resultCode == RESULT_OK) {
                getStr = data?.getStringExtra(Constants.RESULT_MSG).toString()
                bind.shelfNameText.setText(getStr)
            } else {
                getStr = ""
            }

        }
    }
```

onActivityResult 메소드를 오버라이드하여 카메라 엑티비에서 들어온 requestCode를 확인하여 getStringExtra로 값을 받아들입니다.

위 코드에서는 shelfNameText라는 edittext에 setText 메소드로 문자열을 설정했습니다.