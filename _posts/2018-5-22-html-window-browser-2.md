---
layout: post
title: HTML의 윈도우&브라우저 객체에 대하여 알아보기 2
---

오늘도 어제에 이어서 HTML에서 윈도우와 브라우저 관련한 객체에 대하여 간단히 알아보려고 합니다.

## open()

window.open()으로 윈도우를 새로 열고 웹 페이지를 출력합니다.

```javascript
window.open("http://www.google.com", "", "");
```

처음 인자에는 웹페이지의 주소를 전달합니다.
다만, 이 인자에는 null값이 들어갈 수 없습니다.

window 이름을 넣을 수 있는 인자에는 새로 여는 윈도우의 이름을 전달하는 역할을 하며 이 이름은 고유해야 합니다.

만약 기존에 있는 윈도우의 이름이라면 새로운 윈도우 대신에 동일한 이름을 가지고 있는 기존 윈도우에서 페이지가 불러와집니다.

윈도우의 속성을 넣는 인자에는 윈도우의 모양이나 크기와 같은 속성을 전달하는 역할을 합니다.

```javascript
window.open("http://www.google.com", "newWin", "width = 100,height = 100");
```

이름과 속성이 없는 윈도우를 열려면 이름에 null을 넣거나 아에 넣지 않습니다.

```javascript
window.open("http://www.google.com");
window.open("http://www.google.com", null, "");
```

빈 윈도우를 생성하려면 아래와 같이 작성해도 모두 동일하게 작동합니다.

```javascript
window.open();
window.open("");
window.open("", "", "");
window.open("", null, null);
```

## 이름 있는 윈도우와 이름 없는 윈도우의 차이점

이름이 없는 윈도우를 열면 항상 동일한 크기의 새로운 윈도우가 열리면서 사이트를 불러옵니다.

하지만, 이름이 있는 윈도우를 연다면, 해당 이름을 가진 윈도우가 있다면 이미 열려있는 같은 이름의 기존 윈도우에서 주소를 건내주면서 출력합니다.

## close()

윈도우를 닫을 때에 사용하지만, 익스플로러를 제외한 파이어폭스와 같은 브라우저에서는 보안 이유로 작동하지 않습니다.

작동하지 않는 브라우저에서는 윈도우 자기 자신만이 윈도우를 닫을 수 있습니다.

```javascript
var win = window.open();
win.close();
```

> 다음 포스팅에서 계속됩니다.