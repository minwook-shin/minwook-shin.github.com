---
layout: post
title: 코틀린으로 작성한 안드로이드 TextWatcher Event Listener 사용하기
---

오늘은 안드로이드 EditText에서 쓰이는 이벤트 리스너인 TextWatcher를 코틀린으로 작성해보려 합니다.

## 기존 자바 코드

```java
EditText editText = (EditText) findViewById(R.id.editText);
editText.addTextChangedListener(new TextWatcher() {
    @Override
    public void afterTextChanged(Editable p0) {

    }
    @Override
    public void beforeTextChanged(CharSequence p0, int p1, int p2, int p3) {

    }
    @Override
    public void onTextChanged(CharSequence p0, int p1, int p2, int p3) {

    }
});
```

기존에 자바 코드로 작성할 때에는 findViewById로 xml에서 editText라는 아이디를 가져와서 TextWatcher를 TextChangedListener에 추가해주었습니다.

## 코틀린 코드

```kotlin
ediTtext.addTextChangedListener(object : TextWatcher{
    override fun afterTextChanged(p0: Editable?) {

    }
    override fun beforeTextChanged(p0: CharSequence?, p1: Int, p2: Int, p3: Int) {

    }
    override fun onTextChanged(p0: CharSequence?, p1: Int, p2: Int, p3: Int) {

    }
})
```

코틀린 익스텐션의 존재 유무과 코틀린 문법에 따른 차이점으로 인한 것만 아니면 자바 코드와 매우 흡사함을 알 수 있습니다.

## afterTextChanged

입력이 끝날 때 작동됩니다.

## beforeTextChanged

입력 하기 전에 작동됩니다.

## onTextChanged

타이핑 되는 텍스트에 변화가 있으면 작동됩니다.