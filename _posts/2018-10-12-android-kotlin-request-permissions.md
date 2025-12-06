---
layout: post
title: 코틀린 프로젝트로 안드로이드에서 KPermissions 라이브러리 사용해보기
---

오늘은 코틀린 안드로이드 프로젝트에서 KPermissions 라이브러리를 쉽게 사용해보려 합니다.

KPermissions 라이브러리는 권한 추가를 도와주는 (코틀린으로 작성된) 라이브러리입니다.

안드로이드 6.0 이상부터 도입된 강화된 권한 설정을 쉽게 할 수 있습니다.

## 의존성

```java
    implementation 'com.github.fondesa:kpermissions:1.0.0'
```

kpermissions를 사용하기 위해 app.gradle에 추가해줍니다.

## 권한 추가하기

```kotlin
val request = permissionsBuilder(Manifest.permission.CAMERA).build()
request.send()
```

만약 카메라 권한을 추가하려면 위와같이 permissionsBuilder 클래스로 묶어서 빌드해주고, send 메소드를 수행해줍니다.

다른 권한들은 Manifest.permission에 있는 모든 권한들을 넣을 수 있습니다.

## 이벤트 리스너

권한이 허락됬는지, 거부됬는지를 수신받을 수 있습니다.

```kotlin
request.listeners {
    onAccepted { _ ->
        longToast(getString(R.string.all_permission_okay))
    }
    onDenied { _ ->
        longToast(getString(R.string.all_permission_deny))
    }
    onPermanentlyDenied { _ ->
    }
    onShouldShowRationale { _, _ ->
    }
}
```

위와 같이 작성할 수 있고, 아래와 같이 빌더로 작성할 수도 있습니다.

```kotlin
request.onAccepted { permissions ->
}.onDenied { permissions ->
}.onPermanentlyDenied { permissions ->
}.onShouldShowRationale { permissions, nonce ->
}
```

그리고 클래스에서 상속받아 메소드를 오버라이딩하여 아래와 같이 등록할 수도 있습니다.

```kotlin
request.acceptedListener(this)
request.deniedListener(this)
request.permanentlyDeniedListener(this)
request.rationaleListener(this)
```

다 작성하고 빌드를 하면 해당 코드가 위치한 엑티비티에서 권한을 받는 창이 띄워지게 됩니다.