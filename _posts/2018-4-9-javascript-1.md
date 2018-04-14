---
layout: post
title: Javascript에 대하여 알아보기 1
---

오늘은 학교에서 배우고 있는 자바스크립트에 대하여 잠시 알아보겠습니다.

## 이론

자바스크립트는 1995년 Netscape Communication Corporation에서 처음 개발되어 Netscape 브라우저에 탑재되었습니다.

## 조합

자바스크립트는 html5, css3를 조합하여 사용합니다.

## 활용도

사용자의 입력과 계산을 도맡아 하거나, 웹 페이지의 내용 및 모양을 동적으로 제어할 수 있습니다.

또한 브라우저를 유동적으로 제어할 수 있습니다.

웹서버와의 통신을 할 수 있습니다.

## 작성 위치

자바스크립트는 html 태그의 이벤트 리스너 속성에서 작성하거나, 스크립트 태그에 작성하거나 자바스크립트 파일에 작성할 수 있습니다.

1. HTML tag의 event listener 속성에 작성

HTML tag에는 mouse가 click되는 등 event가 발생할 때 처리하는 listener 속성이 있는 것을 활용합니다.

자바스크립트 코드가 짧은 경우에 주로 사용됩니다.

2. ```<script></script>``` tag에 JavaScript 작성

```<head></head>```나 ```<body></body>``` 태그 내 어디든 작성 가능하며, 웹 페이지 내에서도 여러 번 작성가능합니다.

3. 자바스크립트 코드를 별도 파일에 작성

자바스크립트 코드 파일 저장하여 여러 웹 브라우저에서 불러서 사용합니다.

4. URL 부분에 자바스크립트 코드 작성

앵커 태그의 href 속성에서 자바스크립트 코드를 작성할 수 있습니다.

## 문자 출력

JavaScript로 HTML에 내용을 출력할 때는 ```document.write()```를 이용하여 바로 브라우저 창에 띄웁니다.

```document.writeln()```은 문장이 끝난 뒤 ```"\n"```이 됩니다.

다만, 자바스크립트에서는 개행이 되는 것이 아니라, 단지 한칸만 띄어 집니다.

## 대화창

JavaScript Dialogue를 이용하여 사용자 입력 및 메시지 출력을 할 수 있습니다.

사용자에게 메시지를 출력하거나 입력을 받을 수 있는 3가지 dialogue가 있습니다.

* 프롬프트

* 확인

* 경고

프롬프트는 prompt("메시지") 함수로 쓰이며, 

```javascript
var ret = prompt("what your name?");
```

이와 같이 쓰입니다.

확인은 confirm("메시지") 함수로 쓰이며,

```javascript
var ret = confirm("confirm?");
```

이와 같이 쓰입니다.

경고는 alert("메시지") 함수로 쓰이며,

```javascript
alert("warning");
```

이와 같이 쓰입니다.

## 식별자

JavaScript 식별자는 JavaScript 프로그램의 변수, 상수(리터럴 literal), 함수의 이름이며, 식별자 만드는 규칙입니다.

첫번째로 오는 문자는 알파벳과 언더바, $ 문자만 쓸 수 있으며, 그 다음은 숫자도 사용가능합니다.

대소문자는 구별되며, 예약어는 사용 불가합니다.


> 다음 포스팅에서 계속 됩니다.