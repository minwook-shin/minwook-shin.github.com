---
layout: post
title: 안드로이드에서 쓸 수 있는 코틀린 anko에 대하여 알아보기
---

오늘은 안드로이드에서 사용가능한 코틀린 anko 라이브러리를 일부만 알아보려 합니다.

전체 항목은 깃허브에서 존재하는 [anko wiki](https://github.com/Kotlin/anko/wiki)를 방문하시면 자세히 배워가실 수 있습니다.

저는 이 중에서 anko Commons에 포함되있는 Intent와 Dialog 그리고 toast를 알아보려 합니다.

## 설정 방법

```java
dependencies {
    compile "org.jetbrains.anko:anko-commons:$anko_version"
}
```

앱 수준의 build.gradle에 위와 같이 코드를 작성해주고,

```java
ext.anko_version='0.10.1'
```

프로젝트 수준의 build.gradle에 위와 같이 코드를 같이 작성해줍니다.

마지막으로 꼭 sync를 해주어 빌드 과정을 진행합니다.

## Intent

엑티비티를 넘어가는데에 꼭 필요한 부분입니다.

```java
val intent = Intent(this, Activity::class.java)
intent.putExtra("id", value)
intent.setFlag(Intent.FLAG_ACTIVITY_SINGLE_TOP)
startActivity(intent)
```

위와 같은 코드를

```java
startActivity(intentFor<Activity>("id" to value).singleTop())
```

위와 같이 코드를 한줄로 바꿀 수 있습니다.

## Alert

```java
alert {
    customView {
        editText()
    }
}.show()
```

Alert도 마찬가지로 간단하게 구성하고, show()로 바로 보여줄 수 있습니다.

## Toast

```java
toast("toast")
```

일반적인 문자열을 넣어서 toast 메시지를 출력하게 할 수 있습니다. 상대적으로 짧게 출력됩니다.

```java
longToast("longToast")
```

일반적인 문자열을 넣어서 toast 메시지를 길게 출력하게 할 수 있습니다. toast 메소드보다 길게 출력되고 사라집니다.

```java
toast(R.string.msg)
```

리소스에 모아둔 문자열을 getString 메소드 사용없이 바로 이용할 수 있습니다.