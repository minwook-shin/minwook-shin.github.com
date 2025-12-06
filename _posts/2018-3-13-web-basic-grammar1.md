---
layout: post
title: html5 기본 문법 학습하기-1
---

오늘은 학교의 웹 클라이언트 컴퓨팅 과목과 연련되게 웹 페이지 기본 문법을 정리해보려 합니다.

## title

```html
<!DOCTYPE html>
<html>
<head>
<title>제목</title>
</head>
<body>
</body>
</html>
```

브라우저의 타이틀바에 출력되며 head 태그내에서만 작성됩니다.

## 문단 제목

```html
<!DOCTYPE html>
<html>
<head>
<title></title>
</head>
<body>
<h1>1</h1>
<h2>2</h2>
<h3>3</h3>
<h4>4</h4>
<h5>5</h5>
<h6>6</h6>
</body>
</html>
```

문단을 구성하는데 사용하며, h1이 제일 크고 숫자가 커질수록 작아집니다.

글자의 정확한 크기는 브라우저마다 다르며, CSS 스타일로 변경할 수 있습니다.

## tooltip

```html
<!DOCTYPE html>
<html>
<head><title>툴팁 달기</title></head>
<body>
<span title="툴팁.">
텍스트</span>
</body>
</html>
```

페이지의 본문에 마우스가 올라갈 때 설명문이 출력되며, 태그에 해당 속성이 존재하기에 가능합니다.

## 단락

```html
<!DOCTYPE html>
<html>
<head><title></title></head>
<body>
<p>첫번째 단락</p>
<p>두번째 단락</p>
</body>
</html>
```

태그로 단락을 나눌 수 있습니다.

css를 이용하면 단락 단위로 들여쓰기와 내여쓰기를 할 수 있습니다.

단락을 끝내면 다음 문단에 자동으로 한 줄 띄어써줍니다.

## 수평성

```html
<!DOCTYPE html>
<html>
<head><title>수평선</title></head>
<body>
<hr>
</body>
</html>
```

종료 태그가 존재하지 않으며, 이 태그도 css로 굵기나 색상을 변경할 수 있습니다.

## 개행

```html
<!DOCTYPE html>
<html>
<head><title>개행</title></head>
<body>
<br>
<br>
<br>
</body>
</html>
```

개행을 할 수 있는 태그이며, 종료 태그가 없습니다.

## 개발자 태그

```html
<!DOCTYPE html>
<html>
<head><title>개발자 포맷</title></head>
<body>
<pre>
if(int i = 0;i<arr.length;i++){
    system.out.print();
}
</pre>
</body>
</html>
```

이 태그로 감싸면 코드상에 입력한 모습 그대로 출력됩니다.

## 텍스트 스타일

그 외, b 태그로 진하게 할 수 있으며, strong은 b와 비슷하지만 중요한 내용에 씁니다.

em은 강조에 쓰이며, i는 이텔릭체로 강조합니다.

small은 한단계 작은 문자이며, del은 삭제, ins는 추가를 다룹니다.

윗첨자는 sup이고, 아랫첨자는 sub입니다.

mark로 하이라이팅이 됩니다.