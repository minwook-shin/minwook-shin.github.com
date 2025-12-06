---
layout: post
title: HTML event에 대하여 알아보기 2
---

오늘도 어제에 이어서 이벤트 객체에 대하여 간단히 알아보려 합니다.

## 이벤트 객체

이벤트가 발생하면 브라우저는 발생한 이벤트와 관련된 정보를 담은 이벤트 객체를 만들어 이벤트 리스너에게 전달하게 됩니다.

mousedown 이벤트와 같은 경우에는 마우스의 좌표나 버튼의 번호가 담겨있고, keydown 이벤트와 같은 경우는 키의 코드 값이 담겨 있습니다.

## 이벤트 객체 소멸

이벤트가 이어서 발생된다고 해도 브라우저는 한개의 이벤트를 마무리하고 다음 아벤트를 처리하므로 이벤트가 처리되면 이벤트 객체는 소멸됩니다.

## event listener가 event 객체를 전달받는 방법

* 이름을 가진 event listener

```javascript
function f(e) {
}
obj.onclick = f;
```

* 익명 함수

```javascript
obj.onclick = function(e) {
}
```

* 태그를 이용한 event listener

```javascript
function f(e) {
}

<button onclick="f(event)">버튼</button>
```

## 객체 정보

이벤트 객체에서 들어있는 정보는 현재 발생한 이벤트에 대한 다양한 정보를 가지고 있습니다.

* target 속성

이벤트 타겟 객체를 가리키며, 이벤트 타겟은 이벤트를 유발시킨 dom 객체 태그입니다.

* currentTarget 속성

이벤트가 흘러가는 경로 상에 있는 dom 객체중에서 현재 이벤트 리스너를 실행하고 있는 dom 객체를 가리킵니다.

## 디폴트 행동

이벤트의 디폴트 행동이란, 특정 이벤트에 대하여 태그가 수행하는 기본 행동입니다.

이 기본 행동을 없애는 방법은 onclick="return false"를 하여 이벤트를 false로 반환하거나 preventDefault() 메소드를 호출하는 방법이 있습니다.

```javascript
function test() {
    var v = confirm("yes/no");
    return v;
}

<a href="http://www.google.com" onclick="return test()">
```

confirm으로 false 값이 전해지면 false가 반환되어 링크가 수행되지 않습니다.

```javascript
function test(e) {
    e.preventDefault();
}
```

이벤트를 인자로 받아서 메소드를 호출할 수도 있습니다.

## 이벤트 흐름

이벤트가 발생하면 윈도우 객체에 먼저 도달하고 dom 트리를 따라 타겟 객체에 도착합니다.

다시 반대 방향으로 흘러 윈도우 객체에 도달한 다음에 사라집니다.

이벤트가 흘러가는 과정에는 캡쳐 단계와 버블 단계가 있습니다.

* 캡쳐 단계

윈도우에서 타겟 객체까지 이벤트가 전파되는 과정을 의미하며, 중간의 모든 dom 객체를 거쳐갑니다.

실행되거나 거쳐가는 모든 이벤트 리스너가 실행됩니다.

* 버블 단계

다시 타겟 객체에서 거꾸로 윈도우까지 이벤트가 전달되는 과정입니다.

이 역시 중간의 모든 dom 객체를 거쳐가며 이벤트 리스너를 순서대로 실행합니다.

dom 객체에서는 캡쳐 리스너와 버블 리스너 모두 작성할 수 있습니다.

> 다음 포스팅에서 계속됩니다.