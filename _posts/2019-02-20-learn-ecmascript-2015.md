---
layout: post
title: ECMAscript 2015 (es6) 알아보기
---

오늘은 Vue를 실습해보면서 자주 보이는 ECMAscript 2015 (es6)에 대하여 알아보려 합니다.

## jshint setting

자바스크립트 단일파일을 node로 하려니, vscode상에 설치되어있는 jshint가 경고를 출력했습니다.

[스택오버플로우 답변](https://stackoverflow.com/questions/27441803/why-does-jshint-throw-a-warning-if-i-am-using-const/27442276#27442276)을 보니 코드에 ECMAScript 6을 사용하면 파일 상단에 아래와 같이 작성해야 된다고 합니다.

```javascript
/*jshint esversion: 6 */
```

만약 노드 프로젝트라면 .jshintrc 파일에 아래와 같이 입력해줍니다.

```json
{
  "esversion": 6
}
```

## Let

재선언이 불가한 변수입니다.

변수가 메모리에 할당되면 다시 할당할 수 없으므로 다시 선언하려고 하면 already been declared. 라고 오류가 출력됩니다.

```javascript
let b = 20;
console.log("b : " + b);
b = 200;
console.log("b : " + b);
```

## Let Scope

```javascript
let realTotal = 0;
for (let j = 0; j < 10; j++) {
  realTotal++;
  console.log("j : " + j);
}
// console.log("j : " + j);
```

블록 단위로 변수가 제한되게 됩니다.

## Const

한번 선언하면 값 자체를 변경 불가하며 상수의 개념을 가지고 있습니다.

다시 대입하려고 시도하면 Attempting to override 'name' which is a constant. 라고 출력됩니다.

```javascript
const c = 30;
console.log("c : " + c);
// c = 300; Attempting to override 'c' which is a constant.
const arr = [];
arr.push(1);
console.log("arr :" + arr);
```

다만 리스트와 같이 내부가 변경되는 값을 선언하면 내부의 값은 언제든지 추가될 수 있습니다.

## For of

컬렉션 전용의 반복 구문으로서 Symbol.iterator 속성을 가진 컬렉션 객체만 반복할 수 있습니다.

```javascript
let it = [1, 2, 3];
for (let i of it) {
  console.log(i);
}
```

it.val 과 같이 직접 추가한 데이터 필드는 포함되지 않습니다.

## Template string

`` (`) ``으로 문자열을 아래로 내리거나 문자열에 특정 값을 넣을 수 있습니다.

```javascript
let v = "es6";
let hello = `hello ${v}, 
hello, world!`;
```

## Arrows

기존의 function보다 짧게 축약할 수 있으며, 익명 함수로 사용합니다.

```javascript
let f = () => {
  return 404;
};
let list = [1, 2, 3, 4, 5];
list.forEach(value => {
  console.log("value: " + value);
});
```

다만 생성자로서 사용될 수 없습니다.

## Enhanced Object Literal

축약된 문법으로 반복되는 구문을 줄이고, function을 생략할 수 있습니다.

```javascript
let dict = {
  words: 100,
  print() {
    console.log("hello, world!");
  }
};
dict.print();
```

## Generators

제네레이터 함수를 function\*라고 명시하고, yield에 의해 next 메소드가 수행될 때마다 연속적으로 반환합니다.

```javascript
function* gen() {
  yield* ["a", "b"];
}
var g = gen();
console.log(g.next());
console.log(g.next());
console.log(g.next());
```

처음에는 `{ value: "a", done: false }`로 출력되지만, 세번째로 불러오면 `{ value: undefined, done: true }`와 같이 더 이상 불러올 내용이 없기 때문에 값은 undefined이고, 끝났냐는 bool 값은 true로 출력됩니다.

## Import

```javascript
export default function() {}
```

자바스크립트에서 모듈로 내보낼 때 export를 사용합니다.

export default는 모듈 당 하나만 선언됩니다.

## Classes

기존 자바스크립트의 상속보다 클래스답게 사용할 수 있습니다.

```javascript
var Foo = class {
  constructor() {}
  bar() {
    return "Hello World!";
  }
};
var instance = new Foo();
instance.bar();
```

new로 인스턴스를 생성해서 메소드를 수행할 수 있습니다.

## Destructuring

배열 또는 객체를 Destructuring하여 개별적인 변수에 넣는 방식입니다.

```javascript
let [first, second] = [1, 2];
console.log(first);
```

처음 변수를 개별적으로 출력하면 1이 출력됩니다.

## Proxy

Proxy 객체는 대리해주는 객체로서 값은 대입하여 get과 set으로 동작합니다.

```javascript
var p = new Proxy(
  {},
  {
    get: function(target, name) {
      return name in target ? target[name] : 404;
    },
    set: function(obj, prop, value) {
      if (prop === "score") {
        if (value > 100) {
          throw new RangeError();
        }
      }
      obj[prop] = value;
    }
  }
);
p.score = "100";
console.log(p.score);
```

## Promise

비동기 연산을 끝내고 수행할 핸들러를 등록할 때에 사용하며 콜백 함수의 중첩을 막습니다.

```javascript
Promise.resolve(console.log("start")).then(() => {
  console.log("end");
});
```

## 그 외

제가 미처 실습하지 못한 WeakMap, WeakSet이나 심볼과 같은 추가 사항은 http://es6-features.org/#Constants 에 자세히 나와있습니다.
