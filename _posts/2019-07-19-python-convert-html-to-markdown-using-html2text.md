---
layout: post
title: Python HTML 문자열을 마크다운으로 변환하는 html2text 라이브러리 알아보기
---

오늘은 Python에서 HTML 문자열을 마크다운 형식의 문자열로 변환해주는 html2text 라이브러리를 알아보려 합니다.

## html2text 설치

우선 virtualenv로 파이썬 환경을 분리해줍니다.

```
pip3 install virtualenv
```

```
virtualenv -mvenv env
```

env라는 이름의 가상 환경을 생성합니다.

```
source env/bin/activate
```

가상환경을 폴더에서 활성화합니다.

```
pip3 install --upgrade pip
```

pip의 업그레이드가 존재하는지 확인하고 진행합니다.

```
pip install html2text
```

pip로 html2text를 설치합니다.

## 예제

```python
import html2text
```

html2text를 가져옵니다.

```python
html_2_text = html2text.HTML2Text()
```

HTML2Text 객체를 만듭니다.

```html
html = """
<!DOCTYPE html>
<html>
<body>

<h1>First Heading</h1>
<h2>Second Heading</h2>
<h3>Third Heading</h3>
<p>Paragraph.</p>
<b>Bold.</b>
```

Heading과 Paragraph, 그리고 Bold 태그는 아래와 같이 출력됩니다.

```
# First Heading

## Second Heading

### Third Heading

Paragraph.

**Bold.** 
```

```html
<a href="https://www.google.com">google</a>
```

링크는 아래와 같이 출력됩니다.

```
[google](https://www.google.com)
```

```html
<img src="google.jpg" alt="google.com">
```

이미지는 아래와 같이 출력됩니다.

```
![google.com](google.jpg)
```


```html
<ul>
  <li>java</li>
  <li>python</li>
  <li>go</li>
</ul>

<ol>
  <li>java</li>
  <li>python</li>
  <li>go</li>
</ol>
```

리스트는 아래와 같이 출력됩니다.

```
  * java
  * python
  * go

  1. java
  2. python
  3. go
```




```html
<table>
  <tr>
    <th>id</th>
    <th>name</th> 
    <th>e-mail</th>
  </tr>
  <tr>
    <td>test_id1</td>
    <td>test1</td> 
    <td>test@test.com</td>
  </tr>
  <tr>
    <td>test_id2</td>
    <td>test2</td> 
    <td>another@test.com</td>
  </tr>
</table>

</body>
</html>"""
```

테이블은 아래와 같이 출력됩니다.

```
id | name | e-mail  
---|---|---  
test_id1 | test1 | test@test.com  
test_id2 | test2 | another@test.com
```

```python
result = html_2_text.handle(html)
print(result)
```

마지막으로 작성한 html를 파싱하여 마크다운 문자열로 변환되게 됩니다.