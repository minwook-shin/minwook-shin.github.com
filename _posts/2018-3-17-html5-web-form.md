---
layout: post
title: HTML5 웹 폼 배우기
---

오늘은 HTML5에서 웹폼을 만드는 방법을 알아보려합니다.

웹폼이랑 웹 페이지에서 사용자 입력을 받는 형식으로서 로그인아니 검색하는 부분에 사용됩니다.

형식, 즉 form에 구성하는 요소는 input, textarea, select 등이 있습니다.

## 틀

```html
<!DOCTYPE html>
<html> 
<head>
</head> 
<body> 
<form name="form" method="get"> 
ID : <input type="text" value=""><br> 
PW : <input type="password" value=""><br> 
<input type="submit" value="전송"> 
</form>
</body>
</html>
```

이와 같이 form 태그로 둘러싼 다음, input 태그로 입력창과 버튼을 만들어 서로 상호작용을 하게 만듭니다.

type 속성으로 각각 모양이 결정되게 됩니다.

## method

폼 데이터를 웹서버로 전송하는 형식으로서 get과 post가 있는데, 이 둘은 전송 방식의 차이로서 get은 웹 주소의 물음표뒤에 따라 붙고, post는 숨겨져서 보내진다는 큰 차이가 있습니다.

## action

이 속성은 폼 양식의 데이터를 어디에서 처리할 지 웹 서버에 대한 url을 입력하면 됩니다.

## 포털 사이트의 검색 사례

1. 사이트 접속

1. 입력 폼에 검색어를 입력하고 버튼을 누르면 웹 서버에 보낼 엑션을 취합니다.

1. 브라우저는 특정 검색 주소에 접속하여 get 파라미터를 첨부하여 그 사이트에 실행을 요청합니다.

1. 웹 서버에서 실행되어, 검색 결과를 브라우저에 보냅니다.

1. 브라우저는 검색 결과를 화면에 출력합니다.

## 텍스트 입력

```html
<input type="text">
```

한 줄 입력 창을 만들때 쓰입니다.

```html
<input type="password">
```

암호 입력 창을 만들 때 쓰입니다.
사용자 입력 문자 대신 '*' 등 다른 글자로 출력이 됩니다. 이 특수 문자는 속성으로 변경할 수 있습니다. 

```html
<textarea> 
```

여러 줄 입력할 때 쓰입니다.

## 데이터 리스트 

```html
<input type="text" list="test"> 
<datalist id="test"> 
<option value="one"> <option value="two"> <option value="three"> 
</datalist>
```

목록을 리스트화해주는 태그입니다.

과거에는 자바스크립트로 만들었으나, 현재 html5는 태그로 구현할 수 있습니다.

물론 다양한 기능을 구현하려면 여전히 자바스크립트가 필요하긴 합니다.

## 버튼

버튼은 타입에 따라서 하는 일이 다릅니다.

단순한 버튼을 만들 수 있거나, 폼 데이터를 브라우저에 전송하는 submit 버튼, 폼 데이터를 지워주는 reset 버튼이 있습니다.

## 선택 입력

체크박스와 라디오 버튼이 있습니다.

```html
<input type="checkbox">
```

타입 속성을 줘서 체크박스도 만들 수 있습니다.

```html
<input type=“radio”>
```

라디오버튼 역시 타입 속성으로 만들 수 있습니다.

checked 속성을 부여하면 선택된 상태로 출력됩니다.

같은 이름 속성이면 단 한개의 버튼만 누를 수 있습니다.

## 콤보 박스

selet 태그로 작성합니다.

드롭다운 리스트에 목록을 출력하고 선택하는 방식을 나타낼 수 있습니다.

```html
<form> 
<select name="test"> 
<option value="1">non-select</option> 
<option value="2" selected>select</option> 
<option value="3">non-select</option> 
</select> 
</form>
```

선택 입력의 checked 속성과 마찬가지로 selected 속성 또한 먼저 선택 상태가 됩니다.

## 라벨 

label 태그로 캡션과 form 요소를 한 단위로 묶을 수 있습니다.

라벨 태그로 감싸면 그 부분도 같은 폼으로 인식하여 클릭이 됩니다.

## 그룹핑 

fieldset으로 폼 요소를 그룹으로 묶을 수 있습니다.

그룹의 제목은 legend로 표현하며, 외각선 박스에 제목이 출력 됩니다.

```html
<form> 
<fieldset> 
<legend>그룹 제목</legend> 
<input type="email"><br> 
<input type="url"> 
</fieldset>
</form> 
```

위와 같이 구성하면 그룹 제목이 최상단에서 출력되며 그 안에 이메일과 url 폼이 들어가 있습니다.

