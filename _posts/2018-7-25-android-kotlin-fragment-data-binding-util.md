---
layout: post
title: 코틀린으로 작성한 안드로이드 fragment에서 데이터 바인딩 유틸 사용하기
---

오놀은 activity와 다르게 fragment에서 데이터 바인딩 유틸을 사용하여 xml에서 id를 가져와보겠습니다.

## 클래스 생성

```kotlin
class ProductReportFragment : Fragment(){

}
```

android.support.v4.app.Fragment된 Fragment() 메소드를 상속받습니다.

## lateinit

```kotlin
lateinit var bind:FragmentProductReportBinding
```

코틀린에는 lateinit 키워드가 있으므로 초기화를 지연시키면서 전역에 코드를 위치하여 향후 다른 함수들에서도 사용할 수 있게 합니다.

## onCreateView 메소드

```kotlin
override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
}
```

onCreateView 메소드를 오버라이드해줍니다.

fragment에서 사용할 뷰를 만드는 역할을 해주는 메소드입니다.

## DataBindingUtil.inflate

```java
bind = DataBindingUtil.inflate(inflater,R.layout.fragment, container, false)
```

뷰를 얻기위해 바인드를 생성하고 반환합니다.

## 완성 코드

```kotlin
class Fragment : Fragment(){
    lateinit var bind:FragmentBinding
    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        bind = DataBindingUtil.inflate(inflater,R.layout.fragment, container, false)
        return bind.root
    }
    override fun onActivityCreated(savedInstanceState: Bundle?) {
        super.onActivityCreated(savedInstanceState)
    }
}
```