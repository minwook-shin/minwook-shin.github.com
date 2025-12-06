---
layout: post
title: 코틀린으로 안드로이드 Handler 지연사용하기 
---

오늘은 안드로이드 Handler를 이용하여 지연 실행하는 방법을 코틀린으로 해보려 합니다.

## 코드

잠시동안 지연되고 명령을 수행합니다.

```kotlin
Handler().postDelayed({
    //method
}, 2000)
```

postDelayed 메소드에 명령과 지연 시간을 매개변수로 넣어주면 됩니다.

## andorid ktx

안드로이드 OS 패키지에 제공되는 handler를 안드로이드 ktx로 확장할 수 있습니다.

```java
dependencies {
    implementation 'androidx.core:core-ktx:1.0.0-alpha1'
}
```

androidx의 ktx 패키지를 우선 dependencies에 추가해줍니다.

```kotlin
handler.postDelayed(delayInMillis = 2000L) {
    //method
}
```

기존의 코틀린 코드와 별반 다를 것은 없지만, 매개변수로 지연할 시간을 넣어주고 본문 블록에 명령을 넣어주면 됩니다.