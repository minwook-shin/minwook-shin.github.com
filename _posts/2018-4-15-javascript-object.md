---
layout: post
title: Javascript 코어 객체에 대하여 알아보기
---

오늘은 저번에 자바스크립트의 기초를 배운 것처럼 객체에 대하여도 배워보려고 합니다.

## 객체

현실의 객체처럼 자바스크립트에서도 객체의 집합이 있습니다.

객체는 자신만의 고유한 구성 속성이 있으며, 자바스크립트 역시 여러개의 속성과 메소드(함수)로 이루어져있습니다.

## 객체 기반

또한 자바스크립트는 객체기반 언어입니다.

Encapsulation, Inheritance, Polymorphism 등의 특성이 부족하여 객체지향 언어는 아니기 때문입니다.

## 브라우저 제공

개발자가 직접 JavaScript code로 객체를 만들어 사용가능 하지만, 브라우저가 이미 많은 pre-built JavaScript 객체들을 만들어 제공하기도 합니다.

## 객체의 유형

객체의 유형에는 3가지가 있습니다.

* 코어 객체

* DOM 객체

* 브라우저 객체

## 코어 객체

코어 객체는 기본 객체로서 표준입니다.

JavaScript 언어가 실행되는 어디서나 사용 가능한 기본 객체이며, 표준 객체입니다.

Array, Date, String, Math 타입 등이며, 웹 페이지 JavaScript 코드에서, 혹은 서버에서 사용 가능합니다.

코어 객체는 new 키워드로 생성하며,

```javascript
var today = new Date();
```

시간 정보가 담긴 객체를 만들 수 있는 예시입니다.

객체에 접근할 때에는 객체와 맴버 변수나 함수 사이에 점 연산자가 사용됩니다.

배열을 만들 때도 기존의 []로 만드는 것이 있지만, 코어 객체로 만들 수도 있습니다.

```javascript
var arr = new Array("1","2","3","4","5","6","7");
```

초기 값을 가진 배열을 만들 수 있습니다.


```javascript
var arr = new Array(7);
```

크기를 확정하고 선언만 할 수 있습니다.

```javascript
var arr = new Array();
```

크기를 확정하지 않고 선언할 수 있습니다.

이 경우 배열의 크기는 자동으로 늘어납니다.

length 속성으로 배열의 크기를 반환하여 알 수 있습니다.

```javascript
arr.length = 10;
```

또한 length 속성으로 배열의 크기를 임의로 조절할 수도 있습니다.

위와 소개한 것처럼 []로도 배열을 만들 수 있다고 했는데, 사실 이렇게 만들어도 array 객체로 다뤄집니다.

배열에는 정수, 실수, 문자열, 객체, 함수 주소등 여러 타입의 데이터가 섞여서 저장됩니다.

String 객체로 문자열을 담을 수도 있습니다.

string 객체는 생성되면 수정할 수 없습니다.

이 역시 length 속성으로 길이를 읽을 수 있지만, 임의로 수정할 수는 없습니다.

문자열을 배열처럼 나타낼 수도 있습니다.

## DOM 객체

Document Object Model 객체라고 하며, HTML 문서에 작성된 각 HTML 태그들을 객체화한 것들입니다.

HTML 문서의 내용과 모양을 제어하기 위한 목적을 가졌으며, W3C의 표준 객체입니다.

## 브라우저 객체

브라우저의 종류나 크기, 정보를 제공하기 위한 객체로서, 비표준입니다.

## 사용자 객체

사용자가 새로운 타입의 객체를 만들 수 있습니다.

1. new로 오브젝트의 빈 객체를 만들어서

1. 빈 객체에 속성을 추가하고

1. 빈 객체에 메소드를 추가합니다.

```javascript
var person = new Object();

person.name = "name";
person.job = "-";
person.pay = "0";

person.func = func;
```

만약 함수 주소를 넣어줬다면 함수도 구현합니다.

new 키워드로만 사용자 객체를 만들 수 있는게 아니고, 리터럴 표기법으로도 만들 수 있습니다.

```javascript
var person = {
    name : "name",
    job:"-",
    pay:"0",

    func:function(){return 0;}
}
```

이와 같이 중괄호를 이용하여 객체의 속성과 메소드를 저장합니다.

## 프로토 타입

프로토 타입이란 타 언어에서는 class라고 불리는 것입니다.

객체를 생성 시 new 키워드를 이용하며, 타입으로 들어갑니다.

```javascript
function person(name){
    this.name = name;
    this.job = "-";
    this.pay = "0";
}
```

위와 같이 생성자 함수를 만들고,

```javascript
var realPerson = new person("realname");
```

위와 같이 new 키워드를 이용하여 객체를 생성합니다.