---
layout: post
title: Vue.js의 Router view에 대하여 알아보기
---

오늘은 Vue.js 자바스크립트 라이브러리에서의 router view에 대하여 알아보고자 합니다.

여러개의 컴포넌트들을 원하는 위치로 표시하고 싶을 때 router view를 사용해야 합니다.

## router

```html
<html>
  <body>
    <div id="app">
      <router-link to="apple">apple</router-link>
      <router-link to="google">google</router-link>
      <router-view></router-view>
    </div>
  </body>
</html>
```

router-link로 네비게이션을 만들어줍니다.
to props 속성을 이용하여 값을 지정해주면 라우터의 path와 연결해줄 수 있습니다.

router-link는 html 태그로 변환되면 a 태그로 렌더링됩니다.

router-view는 현재 라우터가 제공하는 컴포넌트가 렌더링됩니다.

```html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
```

위 스크립트 주소를 html 파일의 body 태그에 삽입하면 vue와 vue-router를 적용할 수 있습니다.

```javascript
    <script>
      var apple = {
        template: "<div>apple</div>"
      };
      var google = {
        template: "<div>google</div>"
      };
```

인라인 템플릿으로 라우터 컴포넌트를 정의해줍니다.

```javascript
var routes = [
  {
    path: "/apple",
    component: apple
  },
  {
    path: "/google",
    component: google
  }
];
```

라우터를 정의하기 위해서 컴포넌트와 맞춰줍니다.

path로 인하여 router-link의 to 속성에 맞게 컴포넌트로 전달합니다.

만약 vue-cli를 이용한다면 vue 파일을가지고 component에 넣어줍니다.

```javascript
      const router = new VueRouter({
        routes
      });
      new Vue({
        router,
        el: "#app"
      });
    </script>
  </body>
</html>
```

미리 만들어둔 routes를 가지고 라우터 인스턴스를 만들어줍니다.

라우터 인스턴스는 바로 Vue 인스턴스로 들어가게 됩니다.

## 이름있는 뷰

여러개의 컴포넌트를 원하는 위치에 한 화면으로 담으려면 이름이 있는 뷰를 만들어주어야 합니다.

```html
<html>
  <body>
    <div id="app">
      <router-link to="apple">apple</router-link>
      <router-link to="google">google</router-link>
    </div>
  </body>
</html>
```

router-link로 네비게이션을 만들어줍니다.
to props 속성을 이용하여 값을 지정해주면 라우터의 path와 연결해줄 수 있습니다.

router-link는 html 태그로 변환되면 a 태그로 렌더링됩니다.

```html
<router-view></router-view>
```

router-view에 이름이 붙지 않으면 기본적으로 default로 생성됩니다.

```html
      <hr />
      <router-view name="a"></router-view>
      <hr />
      <router-view name="b"></router-view>
    </div>
```

name 속성이 있으면 이름있는 뷰가 됩니다.

```html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
```

위 스크립트 주소를 html 파일의 body 태그에 삽입하면 vue와 vue-router를 적용할 수 있습니다.

```html
    <script>
      var apple = {
        template: `<div>apple<router-view></router-view></div>`
      };
      var google = {
        template: `<div>google<router-view></router-view> </div>`
      };
```

인라인 템플릿으로 라우터 컴포넌트를 정의해줍니다.

이 템플릿에 router-view를 포함하면 nested view로 만들어줄 수 있습니다.

```javascript
var routes = [
  {
    path: "/apple",
    components: { a: apple }
  },
  {
    path: "/google",
    components: { b: google }
  }
];
```

뷰는 기본적으로 컴포넌트의 형태로 렌더링되므로, 한 라우터에 components 옵션으로 여러 컴포넌트를 추가해줍니다.

```javascript
      const router = new VueRouter({
        routes
      });
      new Vue({
        router,
        el: "#app"
      });
    </script>
  </body>
</html>
```

미리 만들어둔 routes를 가지고 라우터 인스턴스를 만들어줍니다.

라우터 인스턴스는 바로 Vue 인스턴스로 들어가게 됩니다.
