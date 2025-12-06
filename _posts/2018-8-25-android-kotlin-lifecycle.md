---
layout: post
title: 코틀린 프로젝트로 안드로이드 lifecycle 패키지를 사용해보기
---

오늘은 코틀린 안드로이드 프로젝트에 androidx 패키지의 lifecycle를 사용해보려 합니다.

## Gradle

```gradle
implementation "androidx.lifecycle:lifecycle-extensions:2.0.0-rc01"
```

먼저 androidx 패키지의 lifecycle-extensions를 추가하여 view model과 lifcycle을 같이 사용할 수 있게 합니다.

## model

```kotlin
data class TestModel(
        val Notifier: MutableLiveData<Int> = MutableLiveData()
)
```

모델은 lifecycle에 존재하는 MutableLiveData 타입의 변수를 만들어줍니다.

## view model

```kotlin
open class MainViewModel(private var count: Int = 0) : ViewModel(),LifecycleObserver{
    val model = TestModel()
    fun increment() {
        model.Notifier.value = ++count
    }
```

MutableLiveData에 count를 증가시켜 반영해주는 함수를 만들어줍니다.

```kotlin
    @OnLifecycleEvent(Lifecycle.Event.ON_RESUME)
    fun resume(){
        if (count != 0){
            increment()
        }

    }
}
```

LifecycleObserver를 상속받아서 생명주기 상태가 ON_RESUME일 때만 resume 함수를 실행합니다.

## view

```kotlin
class AFragment : BaseFragment()
```

```androidx.fragment.app.Fragment```를 import해준 Fragment를 상속받은 BaseFragment 클래스를 상속받아줍니다.

```kotlin
private val viewModel by lazy { ViewModelProviders.of(this).get(MainViewModel::class.java) }
```

ViewModelProviders 메소드로 MainViewModel과 연결해줍니다.

```kotlin
private val observer = Observer<Int> {
    printCount(it)
}
private fun printCount(value: Int) {
    test_text.text = value.toString()
}
```

Observer로 값이 변경되면 알려주는 observer를 만들어서 값을 뷰에 출력되게 만듭니다.

```kotlin
override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
    return inflater.inflate(R.layout.fragment_a,container,false)
}
```

fragment에서 화면을 출력하기 위해 onCreateView에서 inflate 해주며 반환해줍니다.

```kotlin
override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        connectViewModel()
        clickEvent()
    }

    private fun clickEvent(){
        testButton.setOnClickListener {
            viewModel.increment()
        }
    }

    private fun connectViewModel(){
        viewModel.model.Notifier.observe(this,observer)
        lifecycle.addObserver(viewModel)
    }
}
```

fragment에서 뷰가 만들어지면 버튼 클릭 이벤트 리스너를 등록해주고, observe, lifecycle도 연결해줍니다.