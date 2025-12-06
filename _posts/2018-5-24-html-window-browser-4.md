---
layout: post
title: HTML의 윈도우&브라우저 객체에 대하여 알아보기 4
---

오늘은 윈도우 객체의 타이머 기능에 대하여 간단히 알아보려고 합니다.

## setTimeout() / clearTimeout()

윈도우 객체의 타이머를 작동시키는 메소드인 setTimeout()와 clearTimeout() 메소드를 이용하여 타임 아웃 코드를 한번만 실행하는 방법부터 알아보겠습니다.

```html
<script>
var id = setTimeout("Alert(1)",1000 );
</script>
```
첫번째 인자는 타임아웃을 적용할 자바스크립트 코드를 넣고, 두번째 인자에는 타임아웃 지연 시간을 넣습니다.

타임 아웃 지연시간은 밀리초 단위이므로 1000을 넣으면 1초가 됩니다.

```html
<script>
clearTimeout(id);
</script>
```

인자에 타이머를 해제할 객체를 넣으면 타이머가 해제됩니다.

```html
<script>
setTimeout("load('https://www.google.com')", 5000);
</script>
```

5초뒤에 구글 홈페이지가 연결되는 코드입니다.

## setInterval() / clearInterval()

위 setTimeout()와 다르게 타임 아웃 코드를 무한으로 반복할 때에 사용하는 메소드입니다.

```javascript
<script>
var id = setInterval("Alert(1)",1000 );
</script>
```

1초 간격으로 자바스크립트 코드가 반복적으로 호출됩니다.

```javascript
<script>
clearInterval(id);
</script>
```

타이머를 해제합니다.

