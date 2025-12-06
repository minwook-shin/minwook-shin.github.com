---
layout: post
title: 구글의 css 코딩 가이드 알아보기
---

오늘도 이어서 구글에서 제공하는 문서중에 css에서 어떻게 코딩해야되는지 나와있는 문서를 간단히 요약해보겠습니다.

## CSS

* 유효한 css 코드를 사용합니다.

* 의미있는 id나 클래스 명을 사용합니다.

* 의미있지만 가능한 짧게 id나 클래스 명을 사용합니다.

* 타입 selector로 id나 클래스 이름을 한정하지 않습니다.

```css
/* Not recommended */ ul#example {} div.error {}

/* Recommended */ #example {} .error {}
```

* 가능한 단축 속성을 사용합니다.

```css
/* Not recommended */ border-top-style: none; font-family: palatino, georgia, serif; font-size: 100%; line-height: 1.6; padding-bottom: 2em; padding-left: 1em; padding-right: 1em; padding-top: 0;

/* Recommended */ border-top: 0; font: 100%/1.6 palatino, georgia, serif; padding: 0 1em 2em;
```

* 값이 0 이라면 0 뒤에 값을 입력하지 않습니다.

* id와 클래스 명의 단어 구분은 하이픈으로 이루어집니다.

* 모든 블록은 들여쓰기를 합니다.

* 모든 속성의 선언은 항상 세미콜론을 붙이고 끝냅니다.

* 속성과 값 사이에는 공백이 하나 들어갑니다.

* selector와 블록 사이에도 공백이 하나 들어갑니다.

* 새로운 css 규칙은 새로운 두줄로 구분합니다.

* css의 속성에 값을 대입하려면 단일 따움표를 사용합니다.

## 참고 문서

https://google.github.io/styleguide/htmlcssguide.html