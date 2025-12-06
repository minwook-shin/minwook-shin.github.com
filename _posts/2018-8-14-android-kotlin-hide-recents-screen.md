---
layout: post
title: 코틀린으로 작성한 안드로이드 최근 작업 목록에서 앱 화면을 숨기기
---

오늘은 안드로이드에서 매뉴키를 눌러 나타나는 (최근 작업 목록이나 최근 앱으로 불리는) 개요 화면을 숨기는 방법을 알아보려 합니다.

## 테스트 기기 버전

이 포스팅을 위해 사용된 애뮬레이터 및 실기기는 api 버전이 22(안드로이드 롤리팝)에서 28(안드로이드 파이)까지입니다.

## flag

작동하고 싶은 엑티비티에 addFlags 메소드로 FLAG_SECURE를 넣어줍니다.

```java
window.addFlags(WindowManager.LayoutParams.FLAG_SECURE)
```

## base activity

계속 넣어주기 귀찮을 때에는 base 파일을 만들어서 위 코드를 넣은 다음에

```kotlin
open class BaseActivity : AppCompatActivity()
```

BaseActivity 클래스를 작성하고자 하는 모든 엑티비티 클래스에서 상속받으면 됩니다.

## 오버라이드

베이스 엑티비티 클래스에 setContentView 메소드를 오버라이딩하여 XML 파일로 뷰가 만들어 질 때에 addFlags 메소드로 FLAG_SECURE를 추가해줍니다.

```kotlin
override fun setContentView(layoutResID: Int) {
        super.setContentView(layoutResID)
        window.addFlags(WindowManager.LayoutParams.FLAG_SECURE)
}
```

## 화면 캡쳐 방지

만약 addFlags 메소드로 FLAG_SECURE를 추가해준다면, 안드로이드 누가 버전을 기준으로

```
캡쳐화면을 캡쳐하지 못했습니다.
앱이나 조직에서 스크린샷 촬영을 허용하지 않습니다.
```

라고 상단바에 알림이 오면서 화면이 캡쳐되지 않습니다.

(반면 안드로이드 롤리팝 버전으로 테스트하였을 때에는 화면 캡쳐를 막지 못했으므로, 모든 안드로이드 버전이 동일하게 적용되는 것은 아닌 듯 싶습니다.)

