---
layout: post
title: 코틀린으로 작성한 안드로이드 서비스 만들기
---

오늘은 안드로이드 서비스를 코틀린으로 구현해보려 합니다.

## 개요

안드로이드 서비스란 앱의 백그라운드에서 실행되며 오래동안 실행되는 작업을 수월하게 수행할 수 있는 구성 요소입니다.

엑티비티와 차이점이 있다면 별도의 디자인된 UI가 필요 없다는 점이 있습니다.

## service 클래스 파일

```kotlin
class AppService : Service()
```

우선 서비스 클래스를 상속받는 클래스를 만들어 줍니다.

그러면 구현이 필요한 메소드들이 나오게 되는데, 이는 밑에서 서술합니다.

## service 등록

```xml
<service 
    android:name=".AppServiceFile" />
```

앱의 manifest 파일에 서비스를 등록해야 합니다.

application 태그 안에 넣어줍니다.

## service 시작

서비스는 activity나 application에서 시작할 수 있습니다

```java
startService(Intent(context(),AppService::class.java))
```

한 줄로 표현하였지만, 결국은 context와 서비스 클래스 파일을 인자로 받아서 intent로 만든 다음에 startService 메소드로 서비스를 시작해주면 됩니다.

## service 구현

그러면 이제 service에서 구동시킬 내용을 구현해주면 됩니다.

```kotlin
class AppService : Service() {
    override fun onBind(p0: Intent?): IBinder {
        throw UnsupportedOperationException("Not yet")
    }

    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        callEvent()
        return Service.START_STICKY
    }
}
```

처음에 언급했던 구현해야 될 메소드들은 onBind와 onStartCommand이며,

callEvent() 메소드가 있는 부분에 원하는 내용을 넣게 되면 서비스가 시작되면서 작동하게 됩니다.