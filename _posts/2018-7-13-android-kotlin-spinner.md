---
layout: post
title: 코틀린으로 작성한 안드로이드 Spinner 에 대하여 알아보기
---

오늘은 안드로이드 컨테이너에서 있는 spinner에 대하여 알아보려 합니다.

## 상속

```java
class Activity : BaseActivity() , AdapterView.OnItemSelectedListener{
```

우선 상속을 받고, 

```java
override fun onItemSelected(p0: AdapterView<*>?, p1: View?, p2: Int, p3: Long) {
}

override fun onNothingSelected(p0: AdapterView<*>?) {
}
```

상속받은 클래스에서 구현해야 되는 메소드들을 구현할 준비를 합니다.

```java
override fun onItemSelected(p0: AdapterView<*>?, p1: View?, p2: Int, p3: Long) {
        bind.editText.setText(timeSpinnerList[p2])
}

override fun onNothingSelected(p0: AdapterView<*>?) {
}
```

그리고 아이템이 선택되었을 때에 하는 행동을 구현해줍니다.

## spinner 컨테이너 생성

```java
<Spinner
    android:id="@+id/simple_spinner_item"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"/>
```

## spinner 구현

```java
var spinnerList = ArrayList<String>()
spinnerList.add(value.toString())
```

우선 문자열 타입의 ArrayList를 만듭니다.

만든 ArrayList에 원하는 값을 넣어줍니다.

```java
var spinner: Spinner? = null
```

spinner 타입의 변수를 null로 초기화해줍니다.

```java
val arrayAdapter = ArrayAdapter(this@Activity, android.R.layout.simple_spinner_item, spinnerList)
arrayAdapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item)
```

ArrayAdapter를 만들어주어 아까 만든 ArrayList를 넣어줍니다.

```java
spinner = this.bind.datePickSpinner
spinner!!.onItemSelectedListener = this
spinner!!.adapter = arrayAdapter
```

아까 만든 spinner에 bind된 레이아웃 id를 넣어주고, 이 spinner에 ArrayAdapter를 넣어줍니다.