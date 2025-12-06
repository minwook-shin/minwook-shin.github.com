---
layout: post
title: Vue cli 3로 생성된 프로젝트에 컴포넌트 추가하기
---

오늘은 Vue cli 3로 만든 vue 프로젝트에 직접 만든 컴포넌트를 추가해보려고 합니다.

기존 Vue cli 2버전에서 다른 점은 프로젝트를 생성하는 방법과 프로젝트 구조등이 다릅니다.

## vue cli 3 설치

```bash
npm install -g @vue/cli
```

```bash
vue --version
```

2019년 2월 18일 기준으로 3.4.0 버전이 설치됩니다.

## 프로젝트 생성

```bash
vue create add_component
```

터미널에 입력하면 vue CLI 버전이 출력되면서 프로젝트를 프리셋으로 설정하여 생성할 수 있습니다.

3.4 버전 기준으로 default 값은 babel과 eslint가 설정되어 있고, 수동으로 설정할 값은 babel, pwa, router, vuex, scss, linter, unit test, e2e test들로 취사 선택하여 설정할 수 있습니다.

만약 프로젝트를 구성할 때에 놓치더라도 vue add 명령어로 언제든지 추가할 수 있습니다.

## 프로젝트 구조

- /public

index.html이나 favicon과 같이 정적 파일들이 보관되는 폴더입니다.

- /src

대부분의 프로젝트에서 코딩한 내용들은 이 폴더에서 작성하게 되며, 컴포넌트나 컴포넌트에 들어갈 어셋, 라우터, store들을 보관하는 폴더입니다.

- `babel.config.js`

babel의 설정을 관리할 수 있는 파일입니다.

- `package.json`

스크립트들은 vue-cli-service로 제공하므로 그 외의 앱 이름, 버전, 그리고 설명등을 지정할 수 있습니다.

- `README.md`

프로젝트의 전반적인 설명이나 사용법을 기록해두는 마크다운 문서입니다.

- `vue.config.js`

기존 cli에서는 webpack.config.js 파일이 루트 디렉토리에 존재했지만, 3 버전부터는 vue.config.js라는 파일을 루트 디렉토리에 새로 만들어주고 configureWebpack이라는 옵션을 추가해야 합니다. 프로젝트를 생성했을 때에는 기본적으로 생성되어 있지 않습니다.

```javascript
module.exports = {
  configureWebpack: {
    plugins: [new MyAwesomeWebpackPlugin()]
  }
};
```

위와 같이 파일을 configureWebpack으로 생성해두면 설정할 수 있습니다.

## 생성할 컴포넌트 위치

src/components 폴더에 vue 확장자로 원하는 컴포넌트를 만들어줍니다.

## 컴포넌트 생성

컴포넌트를 만들기 위해서 파일을 만들어줍니다.

해당 파일에서 vscode 기준으로 vbase (Base for Vue file)라고 입력하면 아래와 같이 자동 완성으로 입력됩니다.

```html
<template>
  <div></div>
</template>

<script>
  export default {};
</script>

<style scoped></style>
```

dom 트리와 인스턴스를 맵핑하는 template과 데이터와 메소드등을 정의하는 script, 그리고 화면을 꾸며주는 css 문법의 style로 나누어집니다.

template에는 하나의 루트 태그만 허용하며, style에 적혀있는 scoped는 해당 컴포넌트에만 스타일을 한정한다는 의미입니다.

## 컴포넌트 연결

위에서 컴포넌트를 구현했으면 이제 App.vue 파일에서 컴포넌트를 연결해주어야 합니다.

```html
<template>
  <div id="app">
    <NewLocalWorld></NewLocalWorld>
  </div>
</template>

<script>
  import NewLocalWorld from "./components/NewLocalWorld";

  export default {
    name: "app",
    components: {
      NewLocalWorld
    }
  };
</script>

<style></style>
```

만약 구현한 컴포넌트 이름이 NewLocalWorld라면, 위와 같이 script에서 import한 뒤 components 옵션으로 등록하고 커스텀 태그를 등록해주면 됩니다.

```javascript
import NewGlobalWorld from "./components/NewGlobalWorld.vue";

Vue.component("NewGlobalWorld", NewGlobalWorld);
```

만약 전역 컴포넌트로 설정하려면 main.js파일에서 import한 뒤에 Vue.component로 이름을 등록하면 됩니다.

## 실행

```
npm run serve
```

run serve로 웹팩으로 디버그 서버를 구동하여 로컬 서버 테스트를 할 수 있습니다.

```
npm run build
```

run build로 프로덕션 레벨의 컴파일을 수행합니다.
