---
layout: post
title: Vue.js Props와 Life cycle에 대하여 알아보기
---

오늘은 Vue.js 자바스크립트 라이브러리에서의 Life cycle(생명 주기)에 대하여 알아보고자 합니다.

Vue 인스턴스도 객체처럼 관리되기 때문에 Life cycle(생명 주기)은 중요합니다.


## Lifecycle Diagram

vuejs 공식 홈페이지에 있는 Lifecycle 다이어그램 그림입니다.

https://vuejs.org/v2/guide/instance.html#Lifecycle-Diagram

vue의 생명 주기는 크게 create, mount, update, destroy로 나뉩니다.

## create

이 생명 주기는 데이터가 관찰되거나 이벤트가 초기화되기 전이고, html dom 트리도 그려지지 않았습니다.

created 이후에 데이터와 이벤트에 접근할 수 있습니다.

```javascript
new Vue({
    beforeCreate () {
        console.log("beforeCreate");
    },
    created () {
        console.log("created");
    },
```

## mount

템플릿이 컴파일되고 화면이 렌더링되기 직전에 beforeMount가 수행됩니다.

mounted 이후에 컴포넌트와 el에 접근할 수 있습니다.

```javascript
    beforeMount() {
        console.log("beforeMount");
    },
    mounted () {
        console.log("mount");
        this.message = "update";
    },
```

## update

업데이트가 필요한 가상 dom 트리가 다시 그려지기 전에는 beforeUpdate가 수행되고, 다시 그리진 이후에는 updated가 수행됩니다.

```javascript
    beforeUpdate() {
        console.log("beforeUpdate");
    },
    updated () {
        console.log("update");
    },
```

updated와 ```this.$nextTick```의 조합으로 완전히 렌더링되었을 때만 기능을 수행하게 할 수도 있습니다.

## destory

```this.$destroy()```에 의해 강제로 destory시키면 beforeDestroy가 먼저 수행되고 인스턴스를 제거한 다음에 destroyed가 연이어 수행됩니다.

```javascript
    beforeDestroy() {
        console.log("beforeDestroy");
    },
    destroyed() {
        console.log("destroyed");
    },
})
```