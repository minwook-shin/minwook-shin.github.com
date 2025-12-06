---
layout: post
title: 코틀린으로 안드로이드 이메일 공유 만들어보기
---

오늘은 안드로이드 앱을 쓸 때 간혹 나오는 이메일로 공유하는 매뉴를 만들어보려 합니다.

## 전송하고 싶은 이메일 주소 입력

```kotlin
val to = FirebaseUtil.getAuth().currentUser?.email.toString()
val addressees = arrayOf(to)
```

이메일 주소는 string 타입의 문자열을 바로 쓰는 게 아니라, array로 문자열을 묶어줘야 합니다.

## 이메일 제목 입력

```kotlin
val subject = "title"
```

이메일의 제목을 써줍니다.

## 이메일 본문 입력

```kotlin
val message = "message"
```

이메일의 본문에 들어갈 텍스트를 문자열로 넣어줍니다.

## intent 생성

```kotlin
val shareIntent = Intent(Intent.ACTION_SEND)
shareIntent.type = "text/plain"
shareIntent.putExtra(Intent.EXTRA_EMAIL, addressees)
shareIntent.putExtra(Intent.EXTRA_SUBJECT, subject)
shareIntent.putExtra(Intent.EXTRA_TEXT, message)
```

Intent를 생성해주고 타입과 방금만든 변수들을 넣어줍니다.

putExtra는 intent 간에 데이터를 전달할 때에 사용하는 메소드입니다.

## 공유 intent 제목 생성하고 출력

```java
startActivity(Intent.createChooser(shareIntent, "공유"))
```

이제 startActivity 메소드를 이용하여 shareIntent라는 intent를 앱 선택기로 출력하면 됩니다.