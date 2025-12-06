---
layout: post
title: 코틀린으로 안드로이드에서 파이어베이스 데이터베이스 사용하기
---

오늘은 코틀린으로 안드로이드에서 파이어베이스 데이터베이스를 사용하는 방법을 작성해보려 합니다.

이번 포스트에서는 실시간 데이터베이스에 쓰고, addListenerForSingleValueEvent를 이용하여 데이터베이스에서 원하는 child에서 읽는 방식을 서술합니다.

## 설정

앱 수준의 build.gradle 파일에 아래 코드를 붙여 넣습니다.

```java
implementation 'com.google.firebase:firebase-database:16.0.1'
```

build.gradle 파일이 변경되면 최초 한번은 반드시 다시 빌드해주어야 합니다.

## 데이터베이스 쓰기

```java
private val database by lazy { FirebaseDatabase.getInstance() }
private val userRef = database.getReference("users")

userRef.child("email").setValue(user.email)
userRef.child("phone").setValue(user.phoneName)
```

파이어베이스 데이터베이스 객체를 만들어서 users까지 접근합니다.

그리고 child 메소드로 하위 트리 구조에 접근해서 해당 레벨의 키에 값을 지정해줍니다.

## 데이터베이스 읽기

한 번만 호출되고 즉시 삭제되는 이벤트인 addListenerForSingleValueEvent만 실습해보려합니다.

다른 이벤트들도 사용방식을 비슷합니다.

```java
userRef.addListenerForSingleValueEvent(object : ValueEventListener {
        override fun onCancelled(p0: DatabaseError) {
        }
        override fun onDataChange(p0: DataSnapshot) {
            for (snapshot in p0.children) {
                if (snapshot.key.equals("email")) {
                    longToast(snapshot.value)
                }
                else{
                    longToast("Error!!")
                }
            }
        }
    }})
```

구현에 필요한 메소드들을 override로 구현합니다.

onDataChange에 코드를 작성하면, 데이터베이스에 있는 내용이 변경될 때마다 이벤트가 작동하게 됩니다.

반대로 취소 이벤트가 발생하면 onCancelled에 작성된 코드가 실행됩니다.

child로 접근한 경로의 하위 트리구조를 DataSnapshot 형식으로 나타나게 됩니다.

이 DataSnapshot를 for문으로 나누어서 출력할 수 있는 점을 이용하여 원하는 정보를 출력하기 위해서는 snapshot.key로 equals 메소드를 불러 비교하는 코드를 작성합니다.

만약 if문에 맞다면, 아래의 코드가 실행됩니다.

```java
longToast(snapshot.value)
```

longToast는 안드로이드 anko 컴포넌트에 포함된 메소드입니다.

이는 다음 포스트에서 작성하도록 하겠습니다.