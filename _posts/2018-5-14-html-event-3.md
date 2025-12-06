---
layout: post
title: HTML event에 대하여 알아보기 3
---

오늘은 어제에 이어서 캡쳐 리스너와 버블 리스너에 대한 내용을 알아보려합니다.

## addEventListener()

addEventListener 메소드를 사용한다면, 3번째 매개변수로 캡쳐 리스너와 버블 리스너를 판별할 수 있습니다.

세번째 매개변수가 true면 캡쳐 리스너이며, false면 버블 리스너입니다.

```javascript
var btn = document.getElementById("button");
btn.addEventListener("click", c, true);
btn.addEventListener("click", b, false);
```

id가 button인 태그를 찾아서 처음에는 캡쳐 단계로 c함수를 실행하고, 두번째로는 b함수를 버블 단계로 실행합니다.

```javascript
obj.onclick = function(e) {
}
```

addEventListener() 외의 다른 방법의 이벤트 listener를 등록하는 경
우에는 모두 버블 listener로 자동 등록합니다.

## 이벤트 흐름 중단

이벤트가 흘러가는 도중에 임의의 listener에서 이벤트 객체의 stopPropagation() 메소드를 호출하면 이벤트는 더 이상 전파되지 않고 사라집니다.

## 마우스 핸들링

마우스 관련 이벤트 리스너가 호출되는 경우가 있습니다.

* onmousedown : mouse 버튼을 누르는 순간 발생됩니다.

* onmouseup : 눌러진 버튼이 놓여지는 순간 발생됩니다.

* onmouseover : mouse가 태그 위로 올라오는 순간 발생합니다.

* onmouseout : mouse가 태그 위로 벗어나는 순간 발생합니다.
* onmouseenter : mouse가 태그 위로 들어(올라)오는 순간 발생합니다.

* onmouseleave : mouse가 태그 위로 벗어나는 순간 발생됩니다.

* onwheel : HTML 태그에 mouse wheel이 구르는 동안 계속 호출됩니다.

onmouseover과 onmouseenter의 등장은 거의 같습니다.

다만 onmouseenter는 버블 단계가 없으므로, 자식 객체에서 발생한 경우 부모객체에서 처리할 수 없습니다.

## oncontextmenu

사용자가 브라우저의 바탕화면이나 태그위에서 마우스 오른쪽 버튼을 클릭할 때 출력되는 매뉴인 context menu의 기본 기능입니다.

개발자가 oncontextmenu 리스너를 등록하면 이 리스너가 호출됩니다.

이 이벤트를 false로 반환하면 매뉴가 보이지 않습니다.

```javascript
document.oncontextmenu = return false;
```

## onload

웹 페이지가 완전히 불러왔을 때에 호출되는 이벤트 리스너로서 윈도우 객체에서 발생합니다.

윈도우 객체에서 불러들여서 사용할 수 있고,

```javascript
window.onload="alert();";
```

태그에서 onload 리스너를 만들어 사용할 수도 있습니다.

```javascript
<body onload="alert();">
```

img 태그에 의해 생성되는 dom 객체인 이미지 객체는 new image()로도 생성되며, 이미지를 다 불러들여도 onload 이벤트가 발생합니다.

자바스크립트가 실행되고 이미지가 다 불러오지 못했을 수도 있으니 onload 이벤트를 이용하여 다 불러와진 후에 함수를 실행하게 합니다.

```html
<head>
<script>
function
Image() {
    var img = document.getElementById("myImg");
    img.onload = function () {}
}
</script>
</head>
<body onload="Image()">
```

페이지가 불러와지면 함수가 실행되고, 이미지가 불러와지면 익명 함수도 호출합니다.

> 다음 포스팅에서 계속 됩니다.