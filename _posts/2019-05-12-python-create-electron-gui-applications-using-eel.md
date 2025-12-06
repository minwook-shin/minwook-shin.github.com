---
layout: post
title: Python HTML/JS 그래픽 인터페이스 Eel 라이브러리 알아보기
---

오늘은 Python으로 일렉트론처럼 HTML과 자바스크립트로 구현가능한 GUI 그래픽 애플리케이션을 만들 수 있는 라이브러리인 Eel을 적용해보려 합니다.

## Eel 설치

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
pip install eel
```

pip로 Eel을 설치합니다.

## hello, world

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Title</title>
    <script type="text/javascript" src="/eel.js"></script>
    <script type="text/javascript">
      eel.expose(say_hello_js);

      function say_hello_js(x) {
        console.log("Hello " + x);
      }

      say_hello_js("Javascript World!");
      eel.say_hello_py("Javascript World!");
    </script>
  </head>
  <body>
    <p>Hello, World!</p>
  </body>
</html>
```

겉으로 표시될 html 파일을 작성합니다.

자바스크립트의 함수를 파이썬으로 보내거나 파이썬의 함수를 가져올 수 있습니다.

일단 해당 파일을 web 폴더에 저장합니다.

```python
import eel
```

이제 파이썬 파일을 작성하기 위해 eel 패키지를 가져옵니다.

```python
eel.init('web')
```

web이라는 폴더를 불러와서 html, 자바스크립트 파일을 읽어올 준비를 합니다.

```python
@eel.expose
def say_hello_py(x):
    print('Hello %s' % x)
```

해당 함수를 자바스크립트에서 사용할 준비를 합니다.

```python
say_hello_py('Python World!')
eel.say_hello_js('Python World!')
```

파이썬 함수를 바로 사용할 수 있으며, 자바스크립트에서 가져온 함수도 사용할 수 있습니다.

```python
eel.start('helloworld.html', size=(300, 200))
```

web 폴더에 있는 helloworld라는 html 파일을 사용하여 gui를 생성합니다.

## input

```html
<!DOCTYPE html>
<html>
  <head>
    <script
      src="https://code.jquery.com/jquery-3.2.1.slim.min.js"
      integrity="sha384-KJ3o2DKtIkvYIK3UENzmM7KCkRr/rE9/Qpg6aAZGJwFDMVNA/GpGFF93hXpG5KkN"
      crossorigin="anonymous"
    ></script>
    <script type="text/javascript" src="/eel.js"></script>
    <script type="text/javascript">
      $(function() {
        $("#btn").click(function() {
          eel.handle_input($("#inp").val());
        });
      });
    </script>
  </head>

  <body>
    <div>
      <input type="text" id="inp" placeholder="Write anything" />
      <input type="button" id="btn" class="btn btn-primary" value="Submit" />
    </div>
  </body>
</html>
```

겉으로 표시될 html 파일을 작성합니다.

jquery를 사용하여 입력된 데이터를 파이썬 콘솔로 문자열을 보내줍니다.

일단 해당 파일을 web 폴더에 저장합니다.

```python
import eel
```

이제 파이썬 파일을 작성하기 위해 eel 패키지를 가져옵니다.

```python
eel.init('web')
```

web이라는 폴더를 불러와서 html, 자바스크립트 파일을 읽어올 준비를 합니다.

```python
@eel.expose
def handle_input(x):
    print('%s' % x)
```

해당 함수를 자바스크립트에서 사용할 준비를 합니다.

```python
eel.start('input.html', size=(500, 200))
```

web 폴더에 있는 input이라는 html 파일을 사용하여 gui를 생성합니다.
