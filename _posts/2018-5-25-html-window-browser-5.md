---
layout: post
title: HTML의 윈도우&브라우저 객체에 대하여 알아보기 5
---

오늘은 윈도우와 브라우저 객체를 알아보는 마지막 포스팅입니다.

## 윈도우 위치&크기

윈도우의 위치와 크기를 조절할 수 있습니다.

```javascript
window.moveBy(10, 10);
```

윈도우를 위로 10픽셀 움직이고 오른족으로 10픽셀 움직입니다.

```javascript
window.moveTo(10, 10);
```

윈도우의 스크린의 위치를 10,10 좌료로 이동합니다.

```javascript
window.resizeBy(10, -10);
```

윈도우의 크기를 10픽셀 크게, 10 픽셀 좁게 조절합니다.

```javascript
window.resizeTo(20, 30);
```

윈도우 크기를 20*30으로 조절합니다.

이 코드는 익스플로러에서만 된다고 합니다. 참고하시기 바랍니다.

## 페이지 스크롤

```javascript
window.scrollBy(0, 10);
```

스크롤를 위로 10 픽셀 올립니다.

```javascript
window.scrollTo(0, 10);
```

0,20 좌표에 현재 윈도우의 왼쪽 상단 모서리에 출력됩니다.

## 프린트

```javascript
window.print();
```

이 코드가 실행된 뒤에 확인 버튼을 누르면 실제로 페이지가 인쇄됩니다.

## location

윈도우가 열릴 때에 자동으로 생성하며, 윈도우에 불러들인 웹 페이지의 url 정보를 나타내는 객체입니다.

```javascript
window.location = "https://www.google.com";
```

## navigator

현재 작동중인 브라우저에 대한 다양한 정보를 가지는 객체입니다.

## screen

브라우저가 실행되는 screen 장치에 대한 정보를 가지고 있는 객체입니다.

## history

윈도우에서 방문한 웹 페이지 리스트를 나타내는 객체입니다.

```javascript
history.back();
history.go(-1);
```

이전 페이지로 이동할 수 있는 코드 예시입니다.

```javascript
history.forward();
history.go(1);
```

다음 페이지로 이동할 수 있는 코드 예시입니다.