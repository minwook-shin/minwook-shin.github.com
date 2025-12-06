---
layout: post
title: HTML event에 대하여 알아보기 1
---

오늘은 웹 클라이언트에서의 html event에 대하여 알아보겠습니다.

## event

마우스 클릭이나 키보드 입력, 이미지나 html 문서의 로딩, 타이머의 타임아웃 등 사용자의 입력하는 행동이나 문서나 브라우저의 변화를 자바스크립트에게 통지하는 것을 말합니다.

## event listener

발생한 이벤트에 대해 대처하기 위한 자바스크립트입니다.

## 종류

html5 에서 이벤트 종류는 70여개로서 이벤트 리스너 이름은 이벤트 이름 앞에 on이 붙습니다.

문서가 전체적으로 불러왔을 때 쓰이는 load event, 마우스 클릭시 쓰이는 click event, 키보드를 누를 때 사용되는 keypress event 등이 해당됩니다.

## 정의

html 페이지에 작성된 태그들이 브라우저에 의해 dom 객체로 바뀌어 출력되는 것입니다.

## 방법

1. html 태그 내에 작성하거나

1. dom 객체의 event listener property에 작성하거나

1. dom 객체의 addEventListener() 메소드를 이용하는 방법이 있습니다.

## 태그 내에 event listener 작성

listener code가 짧은 경우 사용되며, 

```html
<p onmouseover="this.style.backgroundColor='blue'"></p>
```

이와 같이 사용됩니다.

## DOM 객체의 event listener property에 작성

DOM 객체의 event listener property에 event listener 코드를 작성합니다.

```html
<head>
<script>
function init() {
    var p = document.getElementById("p");
    p.onmouseover = over;
}
function over() {
    p.style.backgroundColor="blue";
}
</script>
</head>
<body onload="init()">
<p id="p"></p>
```

페이지가 완전히 불러오면 onload로 함수를 호출하여 함수에서 p라는 id를 호출해서 이 태그의 이벤트 리스너 속성에 함수를 등록합니다.

## DOM 객체의 addEventListener() method 활용

W3C 표준방법으로서 DOM객체의 addEventListener() method를 이용하여 listener에 등록 가능합니다.

```html
<head>
<script>
function init() {
    var p = document.getElementById("p");
    p.addEventListener("mouseover", over);
}
function over() {
    p.style.backgroundColor="blue";
}
</script>
</head>
<body onload="init()">
<p id="p"></p>
```

페이지가 완전히 불러오면 onload로 함수를 호출하여 이벤트 리스너에 등록합니다.


addEventLister()를 이용하면 동일한 event listener에 여러 함수를 중복하여 등록이 가능합니다.

이 이벤트들을 지우는 방법으로는 removeEventListener()가 존재합니다.

## 익명 함수

익명 함수로 event listener를 작성할 수도 있습니다.

코드가 짧거나 한 곳에서만 사용하는 경우, 익명 함수가 편하기도 합니다.

```html
<head>
<script>
function init() {
    var p = document.getElementById("p");
    p.onmouseover = function () {
        this.style.backgroundColor = "orchid";
        };
}
</script>
</head>
<body onload="init()">
<p id="p"></p>
```

페이지가 완전히 불러오면 onload로 함수를 호출하여 이벤트 리스너에  익명 함수를 등록합니다.

> 다음 포스팅에서 계속됩니다.