---
layout: post
title: Vue.js 컴포넌트에 대하여 알아보기
---

오늘은 Vue.js 자바스크립트 라이브러리에서 HTML 엘리먼트를 확장하여 캡슐화할 때 필요한 전역 컴포넌트와 지역 컴포넌트에 대하여 알아보려합니다.

## CDN 사용하기

```html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

이미 Node.js를 설정해두었지만 간단한 실습해서는 CDN을 사용하여 vue 인스턴스를 실습해보려고 합니다.

위 스크립트 주소를 html 파일의 body 태그에 삽입하면 됩니다.

## 전역 컴포넌트

컴포넌트를 등록하자마자 모든 인스턴스에서 사용 가능하며 Vue.component(id,option)로 등록하게 됩니다.

```html
<body>
    <div id="app">
        <test></test>
    </div>
```

유효 범위를 지정하기 위해 html 태그로 div를 감싸고 id를 app으로 지정합니다. 

test는 Vue 전역 컴포넌트 이름으로 커스텀 태그를 구현한 모습입니다.

```html
    <script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

CDN으로 쉽게 vue를 적용할 수 있습니다.

```html
    <script>
        Vue.component("test", {
            template: '<div>how to use?</div>'
        });
```

Vue 인스턴스를 만들기 전에 전역 컴포넌트를 등록해줍니다.

template 옵션으로 인라인 템플릿을 사용하여 문자열을 출력하게 해줍니다.

```html
        new Vue({
            el: '#app',
        })
    </script>
</body>
```

Vue 인스턴스를 만들면 미리 만들어둔 전역 컴포넌트가 적용됩니다.

그리고 el 옵션으로 app이라는 id를 가진 div 태그를 선택자로 지정해줍니다.

## 지역 컴포넌트

컴포넌트를 원하는 인스턴스에 등록하여 해당 인스턴스의 유효 범위안에서 사용할 수 있으며, components 옵션에서 등록합니다.

```html
<body>
    <div id="app">
        <local-test></local-test>
    </div>
```

html 태그로 div를 감싸고 유효 범위를 지정하기 위해서 id를 app으로 지정합니다. 

local-test는 Vue 지역 컴포넌트 이름으로 커스텀 태그를 구현합니다.

```html
    <script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

CDN으로 쉽게 vue를 적용할 수 있습니다.

```javascript
    <script>
        var local = {
            template: '<div>local!</div>'
        };
```

로컬 컴포넌트를 만들어줍니다.

이 상태로는 아직 Vue 인스턴스에 등록되지 않은 상태입니다.

```javascript
        new Vue({
            components: {
                'local-test': local
            },
            el: '#app',
        })
    </script>
</body>
```

Vue 인스턴스의 components 옵션으로 등록을 하면 local이라는 로컬 컴포넌트는 이 Vue 컴포넌트에서만 사용됩니다.

## 새로운 Vue 인스턴스 추가

```javascript
new Vue({
    el: "#app2",
})
```

만약 위와 같은 새로운 Vue 인스턴스를 만든다면 이전에 만들어둔 전역 컴포넌트는 바로 적용할 수 있지만, 지역 컴포넌트는 별도로 components 옵션에 대입하지 않는 이상 작동하지 않습니다.

전역 컴포넌트와 지역 컴포넌트를 적절히 사용하여 모든 컴포넌트를 무조건 불러오는 것 보다는 각자 다른 범위의 인스턴스에서 사용할 수 있습니다.