---
layout: post
title: 코틀린으로 작성한 안드로이드에서 Firebase Messaging Service 사용하기
---

오늘은 Firebase Messaging Service, 즉 FCM을 코틀린으로 안드로이드 프로젝트에서 써보려 합니다.

## 설정

```java
apply plugin: 'com.google.gms.google-services'
```

google-services를 추가해주고,

```java
implementation 'com.google.firebase:firebase-core:16.0.1'
implementation 'com.google.firebase:firebase-messaging:17.1.0'
```

firebase-core와 firebase-messaging을 dependencies에 추가해줍니다.

## manifest

```xml
<service
    android:name="io.fcm.FirebaseMessagingService"
    tools:ignore="ExportedService">
    <intent-filter>
        <action android:name="com.google.firebase.MESSAGING_EVENT" />
    </intent-filter>
</service>
```

해당 클래스 파일을 manifest에서 서비스 태그를 추가해줍니다.

## class
 
```kotlin
class FirebaseMessagingService : FirebaseMessagingService() {
    override fun onMessageReceived(remoteMessage: RemoteMessage?) {
        if (remoteMessage!!.notification != null) {
            sendNotification(remoteMessage.notification!!.body!!)
        }
    }
    private fun sendNotification(messageBody: String) {
        val defaultSoundUri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION)
        @Suppress("DEPRECATION")
        val notificationBuilder = NotificationCompat.Builder(this)
                .setSmallIcon(R.mipmap.ic_launcher)
                .setContentTitle("title")
                .setContentText(messageBody)
                .setSound(defaultSoundUri)


        val notificationManager = getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
        notificationManager.notify(0, notificationBuilder.build())
    }
}
```

fcm에 의해서 메시지를 수신받고 기기에 알림을 보내는 클래스 파일을 FirebaseMessagingService()를 상속받아 구현해줍니다.

setContentTitle 속성은 푸시 알림의 제목이고, setContentText 속성은 푸시 알림의 본문입니다.

```kotlin
class FirebaseMessagingService : FirebaseMessagingService() {
    override fun onNewToken(s: String?) {
        super.onNewToken(s)
        Log.d("NEW_TOKEN", s)
    }

    override fun onMessageReceived(remoteMessage: RemoteMessage?) {
        super.onMessageReceived(remoteMessage)
    }
}
```

onNewToken 메소드를 오버라이드해 로그에 나오는 토큰 값을 이용하여 파이어베이스 콘솔에서 특정 기기에게 푸시 알림을 줄 수 있습니다.