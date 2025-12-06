---
layout: post
title: Vue 프로젝트에서 서로 독립적인 컴포넌트로 리스트에 아이템 추가해보기
---

오늘은 Vue cli 3로 만든 vue 프로젝트에서 서로 다른 독립적인 컴포넌트에서 상위 인스턴스로 이벤트를 보내어 데이터를 처리하고, 최종적으로 props를 이용하여 데이터를 보여주는 작업을 실습해보려 합니다.

이번 포스팅에서는 input을 담당하는 inputComponent.vue 컴포넌트와 listComponent.vue 컴포넌트로 구성하여 실습하려 합니다.

## vue cli 3 설치하고 프로젝트 생성하기

```bash
npm install -g @vue/cli
```

```bash
vue create add_list
```

터미널에 입력하면 vue CLI 버전이 출력되면서 프로젝트를 생성할 수 있습니다.

## inputComponent 컴포넌트

```html
<template>
  <div>
    <input type="text" v-model="Item" v-on:keyup.enter="addItem" />
    <button v-on:click="addItem">
      add
    </button>
  </div>
</template>
```

우선 입력을 할 수 있는 inputComponent 컴포넌트에 입력 박스와 버튼을 만들어줍니다.

```html
<script>
  export default {
    data: function() {
      return {
        Item: ""
      };
    },
    methods: {
      addItem: function() {
        this.$emit("AddItem", this.Item);
        this.Item = "";
      }
    }
  };
</script>
```

script로는 입력한 데이터를 임시적으로 저장할 data와 이벤트를 보내줄 메소드를 선언해둘 methods를 만들어줍니다.

this.\$emit을 하게되면 상위 컴포넌트에서 이벤트를 감지해서 해당 컴포넌트의 메소드를 실행시키거나 props로 값을 내려줄 수 있습니다.

## listComponent 컴포넌트

```html
<template>
  <div>
    <ul>
      <li v-bind:key="item" v-for="(item,index) in propsData">
        {{item}}
      </li>
    </ul>
  </div>
</template>
```

아이템이 나열될 리스트를 출력할 listComponent 컴포넌트에 li 태그로 리스트의 항목을 구현합니다.

v-for 지시문으로 인하여 상위 컴포넌트에서 받은 props를 반복해서 인덱스마다 item에 순서대로 건내줍니다.

item은 li 태그의 본문에 출력됩니다.

```html
<script>
  export default {
    props: ["propsData"]
  };
</script>
```

리스트 컴포넌트는 단순하게 props를 지정하여 해당 props로 오는 데이터를 받습니다. 이 props의 이름은 propsData라고 예시에는 명시했습니다.

## App 컴포넌트

inputComponent 컴포넌트와 listComponent 컴포넌트의 상위에 존재하는 App 컴포넌트를 만들어서 하나의 데이터를 이벤트와 props를 이용하여 처리해줍니다.

```html
<template>
  <div id="app">
    <input-component v-on:AddItem="addOneItem"></input-component>
    <list-component v-bind:propsData="Items"></list-component>
  </div>
</template>
```

inputComponent와 listComponent를 html 태그상에 배치해둡니다.

v-on 지시문으로 이벤트를 받아서 원하는 메소드로 처리하고, v-bind 지시문으로 Items라는 데이터를 props에 내려 보냅니다.

이렇게 되면 입력을 받으면 이벤트가 발생하고, 이벤트로 인해 해당 App 컴포넌트에서 메소드가 수행되면서 localStorage와 데이터에 값이 쌓이게 됩니다.

```html
<script>
  import inputComponent from "./components/inputComponent";
  import listComponent from "./components/listComponent";

  export default {
    components: {
      Input,
      List
    },
```

components 옵션으로 미리 작성해둔 하위 컴포넌트들을 등록합니다.

위 코드에서는 input을 담당하는 inputComponent 컴포넌트와 listComponent 컴포넌트를 등록했습니다.

```javascript
    data() {
      return {
        Items: []
      };
    },
```

Items는 리스트로 하위 리스트 컴포넌트에 props로 내려 보내게 됩니다.

```javascript
    methods: {
      addOneItem: function(i) {
        localStorage.setItem(i, i);
        this.Items.push(i);
      }
    },
```

addOneItem 메소드는 템플릿에서 작성한 것처럼 input 컴포넌트의 AddItem라는 이벤트 신호로 인하여 수행됩니다.

```javascript
    created() {
      if (localStorage.length > 0) {
        for (var i = 0; i < localStorage.length; i++) {
          if (localStorage.key(i) != "loglevel:webpack-dev-server")
            this.Items.push(localStorage.key(i));
        }
      }
    }
  };
</script>
```

처음 앱이 실행되서 라이프사이클의 created 단계에 다다르면 webpack-dev-server에 관련된 문자열만 빼고 localStorage의 값을 Items에 넣어줍니다.

## 실행하기

```
npm run serve
```

컴포넌트를 다 작성하고, serve로 로컬 서버를 구동할 수 있습니다.
