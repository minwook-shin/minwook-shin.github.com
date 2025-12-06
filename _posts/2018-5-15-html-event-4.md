---
layout: post
title: HTML event에 대하여 알아보기 4
---

오늘은 마지막으로 html 이벤트에 대한 포스팅을 이어가려고 합니다.

## new image()

new image()f로 이미지를 불러오면 img 태그를 사용하지 않고 자바스크립트 코드로 이미지를 생성하여 불러올 수 있습니다.

이를 이미지 동적생성이라고 합니다.

```javascript
var image = new image();
image.src = "img.png"
```

위 코드는 이미지를 객체로 동적 생성하여 이미지를 불러오는 코드입니다.

불러온 이미지를 출력하려면 이미지 태그에 할당된 브라우저의 공간에서 출력할 수 있습니다. 

```html
<img id="Img" src="test.png">
<script>
var Img = document.getElementById("Img");
Img.src = image.src;
</script>
```

## focus

form 관련 이벤트는 key, focus, change가 있으며, focus에는 onblur와 onfocus가 있습니다.

우선 focus는 key의 입력에 대한 독점권이며, 여러개의 창이나 버튼이 있을 때에 마우스로 선택하면 그 항목으로 focus가 옮겨갑니다.

물론 그 전에 잡힌 focus는 사라지게 됩니다.

새로운 focus를 받은 html페이지에서는 onfocus 이벤트 리스너가 호출되며 focus를 잃은 항목에서는 onblur이벤트 리스너가 호출됩니다.

## onchange

select 태그에서 선택된 옵션이 변경되면 onchange 이벤트 리스너를 호출할 수 있습니다.


```html
<script>
function Image() {
    var img = document.getElementById("id");
    img.src = "test.png";
}
</script>

<select id="id" onchange="Image()">
```

태그의 옵션이 변경되면 onchange 이벤트 리스너로 인하여 id 값이 id인 select 태그를 찾아서 이미지를 바꾸어 줍니다.

## key 이벤트

key와 관련된 이벤트 리스너는 onkeydown, onkeypress, onkeyup가 있습니다.

* onkeydown : 키가 눌리는 순간에 호출되어 모든 키에 작동합니다.

* onkeypress : 문자 키와 특정 키에 대해서만 눌러지는 순간에 호출되어 작동합니다.

* onkeyup : 이미 눌러진 키가 다시 돌아왔을 때에 호출되어 작동합니다.

## onreset

리셋 버튼을 클릭할 때에 false를 반환하면 초기화되지 않습니다.

```html
<input type="reset">
<form onreset="return false">
```

## onsubmot

submit 버튼을 클릭할 때에 false를 반환하면 폼에 있는 내용을 전송하지 않습니다.

```html
<input type="submit">
<form onsubmit="return false">
```