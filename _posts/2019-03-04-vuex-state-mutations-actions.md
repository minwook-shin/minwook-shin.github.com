---
layout: post
title: Vuex의 state, mutations, actions 알아보기
---

오늘은 Vue 어플리케이션에서 상태 관리할 수 있는 패턴 라이브러리인 Vuex를 알아보려 합니다.

기존의 컨테이너마다 있는 data, template, method 단반향 데이터 흐름을 store으로 싱글톤처럼 운영하게 바꿀 수 있습니다.

어플리케이션의 상태를 담고있는 store가 변경될 때마다 vue 컴포넌트는 업데이트됩니다.

## Vuex 필요성

상태 관리가 되지 않는다면, 여러개의 컴포넌트가 각자의 데이터를 가지고 통신이 잘 되지 않을 수도 있습니다.

물론 props를 이용해서 할 수 있지만, 상하위 컴포넌트들이 많아지거나 컴포넌트의 관계가 복잡해지면 디버그하기 힘듭니다.

이로 인하여 데이터 통신을 store에 담아 commit하면서 상태를 관리해야 합니다.

## 간단 의미

- state : 상태

- mutations : 상태를 변경할 방법

- actions : 비동기 로직이 포함된 메소드

## vue cli 3 설치하기

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
```

state에 text와 num 속성을 만들어줍니다.

위 예시 코드에서 text 속성은 컴포넌트에서 직접 접근을 하기 위해 만들었고, 실제로 상태를 커밋하면서 진행할 속성은 num입니다.

```javascript
  mutations: {
    addNum(state, payload) {
      state.num += payload.value;
    }
  },
```

state를 관리하는 mutations 속성에 addNum을 추가합니다.

addNum은 payload라는 추가 인자를 가지며, payload.value의 값에 따라 원래 변수에 값을 더해줍니다.

```javascript
  actions: {
    addNum(context, payload) {
      context.commit("addNum", {
        value: payload.num
      });
    }
  }
});
```

mutations 속성으로 값을 넘기고 싶을 때에는 commit할 때에 두번째 인자를 추가해줍니다.

payload.value이므로 value 속성으로 들어가게 됩니다.

commit을 하게 되면 상태가 변경되며 그 이력들이 남겨지게 됩니다.

이는 Vue.js devtools에서 vuex 상태를 추적할 수 있게 된다는 소리입니다.

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

이제 컴포넌트에서 사용하기만 하면 됩니다.

```html
<script>
  export default {
    methods: {
      pushedButton(num) {
        this.$store.dispatch("addNum", { num });
      }
    }
  };
</script>
```

methods 속성에 버튼을 누르면 store의 action을 호출해주는 로직을 추가해줍니다.

```html
<div class="hello">
  <div>{{this.$store.state.text}}</div>
  <div>{{this.$store.state.num}}</div>
</div>
```

state에 직접 접근할 수 있습니다.

```html
  <button v-on:click="pushedButton(10)">up</button>
</div>
```

view 상에서도 버튼을 만들고, 지시문으로 store의 action을 호출해주도록 연결해줍니다.

## 디버그

```
npm run serve
```

serve로 로컬 서버를 구동할 수 있습니다.
