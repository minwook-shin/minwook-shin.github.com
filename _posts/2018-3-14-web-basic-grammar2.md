---
layout: post
title: html5 기본 문법 학습하기-2
---

오늘도 어제에 이어서 웹 페이지 기본 문법을 더 정리해보려 합니다.

## 블록

```html
<!DOCTYPE html>
<html>
<head><title></title></head>
<body>
<hr>
<div style="">
블록 태그입니다.
</div>
</body>
</html>
```

항상 새로운 줄에서 시작하여 출력하며, 양 옆에 다른 콘텐츠를 배치하지 않고, 한 줄을 독점하여 사용합니다.
대표적인 예로 div 태그가 있습니다.

div 태그는 여러 html 코드를 블록으로 묶은 컨테이너입니다.

css를 style 속성에 넣어서 꾸밀 수도 있습니다.

## 인라인

```html
<!DOCTYPE html>
<html>
<head><title></title></head>
<body>
<span>인라인 태그</span>
</body>
</html>
```

블록속에서 삽입되어 블록의 일부로 출력되는 태그입니다.

한 예로는 span 태그입니다.

글씨 일부분에 css를 입히고 싶을 때나 javascript로 제어하고자 할 때 사용합니다.

## 메타 데이터

데이터를 설명하는 데이터로서 사진과 오디오,이미지등을 설명합니다.

## base

```html
<head>
<base href="http://www.google.com">
</head>
```

웹 페이지들의 기본 url을 지정할 수 있습니다.

## link

```html
<head>
<link type="text/css" rel="stylesheet" href="style.css">
</head>
```

외부 파일의 연결을 위해 사용하는 태그입니다.

css나 자바스크립트 파일을 연결해서 직접 연결하는 효과를 받습니다.

## 이미지

이미지는 img 태그의 src 속성에서 이미지 파일의 주소를 지정하여 사용할 수 있습니다.

```html
<!DOCTYPE html>
<html>
<head><title></title></head>
<body>
<img src="media.jpg">
</body>
</html>
```

## 리스트 만들기

ol 과 ul태그가 있습니다.

순서대로 정렬된 리스트와 정렬되지 않은 리스트를 나타냅니다.

```html
<!DOCTYPE html>
<html>
<head>
<title>리스트</title>
</head>
<body>

<ol type="A" >
<li>이 리스트는</li>
<li>순서가 있습니다.</li>
<li>타입 속성으로</li>
<li>스타일을 정합니다.</li>
</ol>

<ul>
<li>순서가 없는</li> 
<li>리스트입니다.</li>
</ul>

</body>
</html>
```

## 표

표를 만들 수 있습니다.

```html
<!DOCTYPE html>
<html>
<head><title></title></head>
<body>

<table>
<caption>캡션</caption>
<thead>
<tr><th>1</th><th>2</th><th>3</th></tr>
</thead>
<tfoot>
<tr><th>7</th><th>8</th><th>9</th></tr>
</tfoot>
<tbody>
<tr><td>4</td><td>5</td><td>6</td></tr>
</tbody>
</table>

</body>
</html>
```

thead는 표의 상단이며 tbody는 표의 본체, 그리고 tfoot는 표의 하단입니다.

## 하이퍼링크

a 태그의 href 속성으로 링크를 만들 수 있습니다.

```html
<!DOCTYPE html>
<html>
<head><title></title></head>
<body>
<a href=“http://www.google.com”></a>
</body>
</html>
```

css로 링크의 색을 지정할 수 있습니다.

## 멀티미디어

html5로 오디오와 비디오를 쉽게 올릴 수 있습니다.

```html
<!DOCTYPE html>
<html>
<head><title></title></head>
<body>
<video src="video.mp4" controls autoplay></video>
</body>
</html>

``` 

```html
<!DOCTYPE html>
<html>
<head><title></title></head>
<body>
<audio src="music.mp3" controls autoplay loop></audio>
</body>
</html>

```

플러그인이 필요없지만, 브라우저나 운영체제에 따라 실행되는 파일 포맷이 약간 다릅니다.