---
layout: post
title: Vuex의 mapState, mapGetters, mapMutations, mapActions 알아보기
---

오늘은 Vue 어플리케이션에서 상태 관리할 수 있는 패턴 라이브러리인 Vuex에 내장된 헬퍼 함수를 알아보려 합니다.

## spread 연산자

우선 es6의 spread 문법을 알고 시작해야 Vuex에 내장된 헬퍼 함수를 이용할 수 있습니다.

```javascript
var arr = [1, 2];
var newArr = [...arr, 3];
console.log(newArr);
```

반복가능한 기존 배열을 새로운 배열에 확장할 수 있습니다.

```javascript
function sum(x, y) {
  return x + y;
}
const num = [1, 2];
console.log(sum(...num));
```

함수도 확장할 수 있습니다.

```javascript
a = {
  hello: "hello",
  world: "world"
};
b = {
  javascript: "javascript",
  ...a
};
console.log(b);
```

b에 a를 확장할 수도 있습니다.

## vue cli 3 설치하기

이제 vue cli 3로 vuex가 포함된 vue 프로젝트를 생성하려 합니다.

```bash
npm install -g @vue/cli
```

위와 같이 진행해야 vue cli 3이 설치됩니다.

## Vue cli 3로 프로젝트 생성하기

```
vue create vuex_test
```

프리셋을 Manually select features으로 선택한 다음에 Vuex를 스페이스바로 선택해줍니다.

```json
"dependencies": {
    "vue": "^2.6.6",
    "vuex": "^3.0.1"
  },
```

vuex가 dependencies에 추가되어 있음을 확인할 수 있습니다.

혹은

```
vue add vuex
```

기존의 vue 프로젝트에 vue cli3로 vuex만 추가할 수도 있습니다.

## store.js

어플리케이션의 상태를 저장할 store를 만들어줍니다.

```javascript
import Vue from "vue";
import Vuex from "vuex";
```

Vue와 Vuex를 가져옵니다.

```javascript
Vue.use(Vuex);
```

Vue 프로젝트에 Vuex를 사용하겠다고 선언해줍니다.

```javascript
export default new Vuex.Store({
  state: {
    text: "hello, world!",
    num: 10
  },
  getters: {
    text(state) {
      return state.text;
    },
    num(state) {
      return state.num;
    }
  },
  mutations: {
    addNum(state, payload) {
      state.num += payload.value;
    }
  },
  actions: {
    addNum(context, payload) {
      context.commit("addNum", {
        value: payload.num
      });
    }
  }
});
```

헬퍼 함수에 넣을 state, getters, mutations, actions 속성을 만들어줍니다.

## main.js

만든 store로 어플리케이션에 등록합니다.

```javascript
import store from "./store";
```

import 구문으로 store 파일을 가져옵니다.

```javascript
new Vue({
  store,
  render: h => h(App)
}).$mount("#app");
```

기존 Vue 객체에 store을 등록해줍니다.

## component.vue

```javascript
import { mapActions } from "vuex";
import { mapGetters } from "vuex";
import { mapMutations } from "vuex";
import { mapState } from "vuex";
```

vuex에서 import로 가져옵니다.

```javascript
methods: {
    ...mapActions(["addNum"]),
    pushedButton(num) {
      this.addNum({
        num: num
      });
    },
    ...mapMutations({ add: "addNum" }),
    pushedButton2(num) {
      this.add({
        value: num
      });
    }
  },
```

addNum이라는 actions을 mapActions으로 template을 간략하게 만들 수 있습니다.

addNum이라는 mutations도 mapMutations으로 template을 간략하게 만들 수 있습니다.

```javascript
computed: {
    ...mapGetters(["text"]),
    ...mapState(["num"])
  }
```

text라는 getters를 mapGetters로 template을 간략하게 만들 수 있습니다.

num이라는 state를 mapState으로 template을 간략하게 만들 수 있습니다.

```html
<template>
  <div class="hello">
    <div>{{text}}</div>
    <div>{{num}}</div>
    <button v-on:click="pushedButton(10)">up</button>
    <button v-on:click="pushedButton2(10)">up</button>
  </div>
</template>
```

template에서 기존에 this.\$store.state.text로 불러오던 것을 text로 할 수 있습니다.

함수도 this.add({value: num})처럼 작성할 수 있습니다.
