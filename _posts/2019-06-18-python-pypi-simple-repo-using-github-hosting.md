---
layout: post
title: Python 패키지 버전 관리를 위한 pypi simple 저장소를 깃허브 페이지로 해결하기
---

오늘은 Python 패키지의 버전관리를 할 수 있는 pypi 서버의 역할에서 simple 저장소를 깃허브 페이지로 호스팅해보려 합니다.

## pep 규칙

https://www.python.org/dev/peps/pep-0503/

pep 503 규칙으로서 simple 저장소는 PyPI의 https://pypi.org/simple 도메인처럼 최상위에 simple 저장소로 명명하게 됩니다.

패키지가 추가되면 URL은 https://pypi.org/simple/(새로운패키지) 로 정해집니다.

그리고 반드시 저장소 내에서 루트 URL은 저장소의 프로젝트 당 하나의 앵커 태그로 href 속성은 특정 프로젝트의 URL에 연결되어야 합니다.

```html
<a href="pytube/">pytube</a>
```

단, 파일이 저장소와 관련하여 호스팅하는 위치에 대한 제약이 없으므로 아래와 같이 패키지의 프로젝트 깃 주소를 사용해도 무방합니다.

```html
<a href="git+https://github.com/minwook-shin/pytube@master#egg=pytube-9.5.1">pytube-9.5.1</a><br>
```

그 외에도 gpg 서명, 파이썬 버전 명시에 대한 내용도 있습니다.

## 실습

simple이라는 폴더를 만듭니다.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Simple index</title>
</head>
<body>
    <a href="pytube/">pytube</a>
</body>
</html>
```

위 코드를 index.html로 만들면 simple 저장소 디렉토리의 루트는 완성했습니다.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>pytube</title>
</head>
<body>
    <a href="git+https://github.com/minwook-shin/pytube@master#egg=pytube-9.5.1">pytube-9.5.1</a>
</body>
</html>
```

이제 세부적으로 특정 패키지의 페이지를 만들어줍니다.

simple/패키지명/index.html로 만들어 줍니다.

위 예제의 pytube는 원 프로젝트에서 한 이슈를 해결한 커밋이 남아있는 포크 프로젝트입니다.

```
python3 -m http.server
```

파이썬으로 http 서버를 수행할 수 있도록 터미널에 입력합니다.

```bash
pip3 install --extra-index-url (url)/simple pytube==9.5.1
```

http 서버가 수행되고, 패키지를 설치할 컴퓨터의 터미널에서 위와 같이 명령어를 입력하게 되면, 루프백 아이피의 simple 저장소를 접근해서 특정 패키지를 설치하게 됩니다.

이 때 서버를 찾지 못하거나, pypi 서버에 존재하는 패키지의 버전이 더 높다면 pypi.org에서 패키지를 찾아 설치됩니다.

## 깃허브 업로드

깃허브에 저장소를 만들고, 기존에 github page를 만들 때처럼 진행하면 됩니다.

만약 github page에 대해서 처음 듣고, 하는 방법을 모르겠으면 아래 url을 참고해주세요.

https://pages.github.com

위 주소에서 설명하는 것을 3줄 요약하자면, 아래와 같습니다.

1. 저장소 만들기
2. 저장소 클론받기
3. 작성한 html 파일 커밋 & 푸시하기

접속할 주소는 저장소의 이름에 따라서 (본인닉네임).github.io 이거나 (본인닉네임).github.io/(저장소 이름)입니다.

## 단점

pip search는 simple 저장소를 쓰지 않고, post 요청으로 처리하기 때문에 정적 사이트만 지원하는 깃허브 페이지에서는 사실상 pip install만 사용할 수 있습니다.

그래서 서버 비용없이 흩어져 있는 패키지를 한 곳에 모아 관리하겠다고 하면 좋을지도 모르지만, 파일의 호스팅과 터미널에서의 검색도 필요하다면 pypi 서버를 운영하는게 좋을 것 같습니다.


## 마무리

사실 위와 같이 하면 앵커 태그를 하드코딩으로 처리한 것이라 실제로 서비스에 적용하기에는 무리가 있습니다. 

그래서 저와 같은 경우에는 깃허브 API와 jekyll을 사용해서 하드 코딩된 요소를 삭제해보려 노력하고 있습니다.