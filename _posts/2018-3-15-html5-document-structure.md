---
layout: post
title: 웹 클라이언트 HTML5 문서 구조화
---

오늘은 html5 문서 구조화와 웹 폼에 대하여 정리해보려합니다.

## html5의 문서 구조

html5의 문서 구조는 우선 기존 html이 웹 문서 구조를 표현하는 태그가 없다는 한계를 극복해낸 구조입니다.

문서 구조화를 해야 하는 이유는 검색엔진이 좋아하는 웹 페이지를 작성하기 위한 필요성이 나타나면서 중요해졌습니다.

## 시맨틱 태그

그리하여 html5에서 지원되는 문서의 구조와 의미를 전달할 시맨틱 태그가 있습니다.

예를 들어 head, section, article, main, summary, mark, time이 있습니다.

시맨틱 웹이란 웹 문서를 구조화하여 의미 있는 내용을 탐색하는 웹을 시맨틱 웹이라고 합니다.

## header

페이지나 섹션의 머릿말입니다.

페이지의 제목와 페이지의 소갯말을 작성합니다.

기존 html 태그인 head와는 다릅니다.

## nav

네비게이션의 줄임말로서 하이퍼링크를 모아둔 섹션입니다.

페이지 내의 목차를 만들 때 쓰입니다.

## section

문서의 장이나 절을 구성하는 역할을 하비다.

보통 여러 섹션을 구성할 수 있으며, 헤딩 태그를 이용하요 주제를 기입할 수 있습니다.

## aricle

독립적인 컨텐츠를 담을 때의 영역입니다.

섹션에 비해 주로 보조적인 역할을 맡습니다.

## aside

본문에서 약간 벗어난 번외 노트를 작성할 때 쓰입니다.

페이지의 양 옆에 배치 된다고 합니다.

## footer

주로 저작권에 대한 정보를 기입합니다.

## 모양

시맨틱 태그는 위치와 색, 모양이 자동으로 맞춰서 결정되지 않습니다.

개발자가 직접 css를 이용하여 지정해주어야 합니다.

```html
<!DOCTYPE html> 
<html> 
<head>
<style>
.header { width: 100%; height: 15%; background: blue; } 
.nav { width: 15%; height: 70%; float: left; background: red; } 
.section { width: 70%; height: 70%; float: left; background: green; } 
.aside { width: 15%; height: 70%; float: left; background: red; } 
.footer { width: 100%; height: 15%; clear: both; background: black; }
</style> 
</head> 
<body> 
<header class="header">header</header> 
<nav class="nav">nav</nav> 
<aside class="aside">aside</aside> 
<section class="section">section</section> 
<footer class="footer">footer</footer> 
</body> 
</html>
```

## figure

그림을 블록화하게 해주는 시맨틱 태그입니다.

그림제목은 figure내의 figcaption 태그로 작성합니다.

## detail

detail 태그와 추가로 summary 태그를 이용하여 핸들로 내용을 숨기거나 보이게 할 수 있습니다.

## 그 외

mark 태그는 중요한 텍스트임을 표시하고, time은 텍스트 내용이 시간임을 표시하고, meter 태그는 주어진 범위나 퍼센트의 양을 표현합니다.

마지막으로 progress는 작업의 진행도를 나타내는데 쓰입니다.
