---
layout: post
title: 코틀린으로 작성한 안드로이드 서비스와 파이어베이스 실시간 데이터베이스를 연결해보기
---

오늘은 안드로이드 서비스와 구글의 파이어베이스 실시간 데이터베이스를 같이 합쳐서 앱에 실시간으로 데이터를 갱신해주는 코드를 코틀린으로 구현해보려 합니다.

## 개요

안드로이드 서비스란 앱의 백그라운드에서 실행되며 오래동안 실행되는 작업을 수월하게 수행할 수 있는 구성 요소이며, 이를 이용하여 파이어베이스의 이벤트 리스너로 앱 데이터와 클라우드 데이터를 동기화해줄 수 있습니다.

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

## service 구현

파이어베이스 실시간 데이터베이스에는 ChildEventListener라는 이벤트 리스너로 child가 변경되었을 때마다 코드를 수행하게 할 수 있습니다.

```kotlin
class AppService : Service() {
    override fun onBind(p0: Intent?): IBinder {
        throw UnsupportedOperationException("")
    }
    fun getRef(ref: String): DatabaseReference = database.getReference(ref)

    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        loadAllUserData()
        return Service.START_STICKY
    }
    private fun loadAllUserData() {
        FirebaseUtil.getRef("user").addChildEventListener(object : ChildEventListener {
            override fun onChildRemoved(p0: DataSnapshot) {
                App.INSTANCE.userMap.remove(p0.key.toString())
            }
            override fun onChildChanged(p0: DataSnapshot, p1: String?) {
                val tmpUser = User()
                for (snapshot in p0.children) {
                    if (snapshot.key == "email") {
                        tmpUser.userEmail = snapshot.value.toString()
                    }
                    if (snapshot.key == "userName"){
                        tmpUser.userName = snapshot.value.toString()
                    }
                }
            }
            override fun onChildAdded(p0: DataSnapshot, p1: String?) {
                val tmpUser = User()
                for (snapshot in p0.children) {
                    if (snapshot.key == "email") {
                        tmpUser.userEmail = snapshot.value.toString()
                    }
                    if (snapshot.key == "userName"){
                        tmpUser.userName = snapshot.value.toString()
                    }
                }
            }
            override fun onCancelled(p0: DatabaseError) {
            }
            override fun onChildMoved(p0: DataSnapshot, p1: String?) {
            }
        })
    }
}
```

서비스가 시작되면 onStartCommand 메소드가 수행되면서 예시코드에서는 loadAllUserData 메소드가 수행됩니다.

loadAllUserData 메소드에는 특정 레퍼런스의 위치에 ChildEventListener를 addChildEventListener로 등록해주었습니다.

새로 child가 생기거나 변경되거나 취소되는 등의 이벤트가 발생하면 이벤트 리스너가 작동하게 됩니다.

이렇게 이벤트 리스너를 안드로이드 서비스에 등록해둔다면 앱에서 데이터를 로컬 변수로만 통신할 수 있게 됩니다.