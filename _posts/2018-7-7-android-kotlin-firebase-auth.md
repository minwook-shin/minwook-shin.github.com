---
layout: post
title: 코틀린으로 안드로이드에서 파이어베이스 인증 사용하기
---

오늘은 코틀린으로 안드로이드에서 파이어베이스 인증을 사용하는 방법을 작성해보려 합니다.

이번 포스트에서는 파이어베이스 인증에서 로그인 방식을 이메일과 비밀번호만 이용하는 것을 가정하고 설명합니다.

## 설정

앱 수준의 build.gradle 파일에 아래 코드를 붙여 넣습니다.

```java
implementation 'com.google.firebase:firebase-auth:16.0.2'
```

build.gradle 파일이 변경되면 최초 한번은 반드시 다시 빌드해주어야 합니다.

## 회원 가입

```java
private val mAuth: FirebaseAuth by lazy { FirebaseAuth.getInstance() }
mAuth.createUserWithEmailAndPassword(email, password)
    .addOnCompleteListener {
        var intent = Intent(this, MainActivity::class.java)
        intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP)
        startActivity(intent)
    }
    .addOnCanceledListener {
        alert("Retry", "Wrong ID/PASSWORD") {
            yesButton { }
        }.show()
    }
    .addOnFailureListener {
        alert("Retry", "Wrong ID/PASSWORD") {
            yesButton { }
        }.show()
    }
```

파이어베이스의 인증과 관련된 객체를 만들어서 createUserWithEmailAndPassword 메소드를 부릅니다.

가입에 성공했을때와 취소되었을 때, 그리고 가입에 실패했을 때에 대한 세가지의 이벤트리스너 메소드를 작성합니다.

저는 addOnCompleteListener 위주로 작성하였습니다.

```java
var intent = Intent(this, MainActivity::class.java)
intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP)
startActivity(intent)
```

가입에 성공하면 MainActivity로 넘어가는 코드를 addOnCompleteListener에 추가하여 성공 시에 가입창은 사라지고, 다시 메인 화면으로 돌아가게 됩니다.

만약 가입에 실패(이메일 주소 형식에 안 맞거나, 비밀번호가 틀릴 때)하면 addOnFailureListener가 불러지게 됩니다.

## 로그인

```java
private val mAuth: FirebaseAuth by lazy { FirebaseAuth.getInstance() }
mAuth.signInWithEmailAndPassword(email, password)
    .addOnCompleteListener {
        if (it.isSuccessful) {
            var intent = Intent(this, MainActivity::class.java)
            intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP)
            startActivity(intent)
        }
    }
    .addOnCanceledListener {
        alert("Retry", "Wrong ID/PASSWORD") {
            yesButton { }}.show()
        }
    .addOnFailureListener {
        alert("Retry", "Wrong ID/PASSWORD") {
        yesButton { }}.show()
    }
```

로그인은 signInWithEmailAndPassword 메소드를 사용하며, 성공하면 addOnCompleteListener 이벤트 리스너가  실행됩니다.

로그인도 성공하면 현재 화면을 지우고, 메인 화면으로 넘어가는 코드를 작성했습니다.

## 사용자 조회

```java
private val mAuth: FirebaseAuth by lazy { FirebaseAuth.getInstance() }
user = mAuth.currentUser?.uid.toString()
```

만약 사용자가 앱에 로그인되어 있다면, 해당 사용자의 uid가 표기되고, 로그인되어있지 않으면 null이 표기됩니다.