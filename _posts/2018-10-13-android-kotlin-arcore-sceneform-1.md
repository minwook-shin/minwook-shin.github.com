---
layout: post
title: 코틀린 프로젝트로 안드로이드에서 ARcore sceneform 사용해보기 1
---

오늘은 코틀린 안드로이드 프로젝트에서 ar을 쉽게 구현할 수 있는 라이브러리인 arcore sceneform을 쉽게 사용해보려 합니다.

## 자바8 이슈

```java
android {
    compileOptions {
        sourceCompatibility 1.8
        targetCompatibility 1.8
    }
}
```

Sceneform 라이브러리가 자바8으로 작성되어 있어서 minSdkVersion가 26 이하면 위와 같이 app.gradle에 작성해주어야 합니다.

## 의존성

```java
dependencies {
    implementation 'com.google.ar:core:1.5.0'
    implementation 'com.google.ar.sceneform:core:1.5.0'
    implementation 'com.google.ar.sceneform.ux:sceneform-ux:1.5.0'
}
```

ux 의존을 없앤 버전을 사용하려면 core만 작성해주시면 되지만, 일반적으로는 sceneform-ux 를 위와 같이 app.gradle에 작성해주어야 합니다.

## manifest

```xml
<uses-permission android:name="android.permission.CAMERA" />
<uses-feature android:name="android.hardware.camera" />
```

manifest 파일에 카메라 관련 설정을 추가해줍니다.

안드로이드 6.0 이상에서는 카메라 권한을 추가로 요청해야 합니다.

해당 작업은 이전에 작성한 [권한에 대한 포스팅](https://minwook-shin.github.io/android-kotlin-request-permissions/)을 참고하시면 됩니다.

```xml
<application>
    <meta-data android:name="com.google.ar.core" android:value="required" />
</application>
```

ar에 대한 설정을 application에 넣어주면 됩니다.

required로 되어 있으면, play store에 앱이 등록될 때 이 앱은 arcore가 있어야 한다고 자동으로 인지할 수 있습니다.

만약 ARcore가 꼭 필요없는 앱이라면 optional로 변경해주면 됩니다.

## 런타임 확인

```kotlin
val availability = ArCoreApk.getInstance().checkAvailability(this)
        if (availability.isTransient) {
            Handler().postDelayed({
                maybeEnableArButton()
            }, 200)
        }
camera_fab.isEnabled = availability.isSupported
```

만약 ARcore가 꼭 필요없는 앱이라서 optional로 변경해주었다면, 앱에서 ar이 동작할 때 arcore가 동작하는지 확인해주어야 합니다.

## ARcore 앱 감지

gradle에서 Sceneform을 의존성으로 걸었다면 앱에서 arfragment를 생성할 때에 자동으로 앱이 있는지 확인해줍니다.

```kotlin
private var installRequested: Boolean = false
private var session: Session? = null

override fun onResume() {
    super.onResume()
    if (session == null) {
        var exception: Exception? = null
        var message: String? = null
        try {
            when (ArCoreApk.getInstance()?.requestInstall(this, !installRequested)) {
                ArCoreApk.InstallStatus.INSTALL_REQUESTED -> {
                    installRequested = true
                        return
                }
                ArCoreApk.InstallStatus.INSTALLED -> {
                }
            }
            session = Session(this)
        } catch (e: UnavailableArcoreNotInstalledException) {
                message = "INSTALL_REQUESTED ARCore"
                exception = e
        }
        if (message != null) {
                Log.e(TAG, "Exception", exception)
                return
        }
    }
    session!!.resume()
}
```

세션을 시작할 때에 예외처리를 통해 arcore를 설치했는지 판단할 수 있습니다.

## openGL 버전 확인

arcore는 기본적으로 스마트폰이 openGL 3.0 이상을 지원해야 가능합니다.

```kotlin
val openGlVer = (this.getSystemService(Context.ACTIVITY_SERVICE) as ActivityManager)
        .deviceConfigurationInfo
        .glEsVersion
if (openGlVer.toDouble() < MIN_OPENGL_VERSION) {
    longToast("requires OpenGL ES 3.0 later!")
} else {
    // ARfragment 실행
}
```

시스템의 openGL 버전을 가져와서 소수점이 필요하므로 더블형으로 변환해주며 MIN_OPENGL_VERSION를 비교해봅니다.

```kotlin
private val MIN_OPENGL_VERSION = 3.0
```

MIN_OPENGL_VERSION는 미리 지정한 상수입니다.

비교했을 때에 지원되는 버전 이상이면 ARfragment를 나타나게 해주는 엑티비티로 이동하게 됩니다.

> 다음 편에서는 sceneform에서 ArFragment를 생성하고, 모델링을 나타나게 해보겠습니다.