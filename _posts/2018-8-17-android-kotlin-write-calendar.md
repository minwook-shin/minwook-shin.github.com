---
layout: post
title: 코틀린으로 작성한 안드로이드 Calendar를 사용하여 현재 날짜 구하기
---

오늘은 안드로이드에서 Calendar 클래스를 사용하여 현재 날짜를 구해보는 방법을 코틀린으로 작성해보려 합니다.

## 객체 생성

```kotlin
val instance = Calendar.getInstance()
```

Calendar 클래스의 객체를 만들어줍니다.

## 현재 년도

```kotlin
val year = instance.get(Calendar.YEAR).toString()
```

Calendar 객체의 년도 상수를 얻어서 변수에 넣습니다.

## 현재 월

```kotlin
val month = instance.get(Calendar.MONTH) + 1).toString()
```

Calendar 객체의 월 상수를 얻어서 변수에 넣습니다.

## 현재 일자

```kotlin
val date = instance.get(Calendar.DATE).toString()
```

Calendar 객체의 일자 상수를 얻어서 변수에 넣습니다.

## 그 외

WEEK_OF_YEAR

현재 년도의 주

WEEK_OF_MONTH

현재 월의 주

DAY_OF_YEAR

현재 년도의 날짜

DAY_OF_MONTH

현재 달의 날짜

DAY_OF_WEEK

현재 요일

## 한 자리수 처리

```kotlin
if (monthStr.toInt() < 10) {
    monthStr = "0$monthStr"
}
if (dateStr.toInt() < 10) {
    dateStr = "0$dateStr"
}
```

만약 월과 날짜가 한 자리수이면 0을 붙혀줍니다.

안드로이드 스튜디오 ide에서 Convert concatenation to template라는 옵션을 선택하면 위와 같이 달러 기호를 사용하여 작성할 수 있습니다.

## 코드

```kotlin
fun timeGenerator() :String{
    val instance = Calendar.getInstance()
    val year = day.get(Calendar.YEAR).toString()
    var month = (day.get(Calendar.MONTH) + 1).toString()
    var date = day.get(Calendar.DATE).toString()
    if (monthStr.toInt() < 10) {
        monthStr = "0$monthStr"
    }
    if (dateStr.toInt() < 10) {
        dateStr = "0$dateStr"
    }
    return yearStr + monthStr + dateStr
}
```

위와 같이 작성하면 "20180817"이라는 String 타입의 문자열이 반환됩니다.
