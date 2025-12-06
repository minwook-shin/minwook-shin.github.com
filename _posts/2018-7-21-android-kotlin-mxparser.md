---
layout: post
title: 코틀린으로 작성한 안드로이드에서 mXparser 라이브러리 사용하기
---

오늘은 문자열로 이루어진 수학 수식들을 계산해주는 라이브러리인 mXparser를 gradle로 추가하고 코틀린 프로젝트에서 사용해보았습니다.

## 설정 

```java
implementation 'org.mariuszgromada.math:MathParser.org-mXparser:4.2.0'
```

## 사용

```kotlin
val e1 = Expression("2018-7-21*100")
bind.Text.setText(Math.round(e1.calculate().toString().toDouble()).toString())
```

Expression 메소드 인자에 문자열 타입으로 식을 넣습니다.

calculate() 메소드로 계산을 하면 기본적으로 소수점이 붙어서 나오므로 Math 패키지에 있는 round 메소드로 반올림하고 다시 toString으로 계산한 결과를 출력하게 도와줍니다.

## 콘솔에 출력하기

```kotlin
val a = Expression("2018^7-21*100");
mXparser.consolePrintln(a.calculate().toString());
```

mXparser에서 콘솔 출력을 consolePrintln 메소드로 제공해줍니다.