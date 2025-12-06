---
layout: post
title: Python 웹 애플리케이션 자동으로 테스트하는 selenium 라이브러리 알아보기
---

오늘은 Python에서 웹 애플리케이션을 자동으로 테스트할 때 사용할 수 있고, 자바스크립트가 불러와진 다음의 내용을 크롤링하게 할 수도 있는 selenium 라이브러리를 알아보려 합니다.

## selenium 설치

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
pip install -U selenium
```

pip로 selenium을 설치합니다.

```python
pip install beautifulsoup4
```

추가로 아래 실습에서는 보여지는 화면에서 문자열을 가져올 예정이므로 beautifulsoup4도 설치합니다.

## 드라이버 설치

chrome: https://sites.google.com/a/chromium.org/chromedriver/downloads

Edge: https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/

Firefox: https://github.com/mozilla/geckodriver/releases

chrome, Edge, Firefox는 브라우저 드라이버를 내려받아야 합니다.

Safari:	사파리의 환경설정에서 개발자용 매뉴를 활성화한 다음, "원격 자동화 허용" 옵션을 활성화합니다.

## 구글에서 날씨 가져오기

```python
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
import time
from bs4 import BeautifulSoup
```

selenium과 time, 그리고 BeautifulSoup을 가져옵니다.

selenium으로 웹 브라우저를 구동해서 원하는 결과가 랜더링될 때까지 time으로 조금 기다리고, 마지막으로 BeautifulSoup로 문자열을 가져올 수 있게 합니다.

```python
browser = webdriver.Safari()
```

사파리 브라우저의 드라이버로 시작합니다.

이 부분은 크롬이나 파이어폭스로 바꿔서 해도 됩니다.

```python
browser.get('http://www.google.com')
```

구글 사이트로 접속합니다.

```python
browser.find_element_by_name('q').send_keys("weather", Keys.RETURN)
```

q 라는 이름의 입력 태그에 접근해서 weather라는 단어를 치고, 앤터 키를 누릅니다.

```python
time.sleep(2)
browser.save_screenshot("test.png")
```

잠시 쉬고, 현재 브라우저 화면을 test라는 사진 파일을 만듭니다.

time 패키지 대신에 driver.implicitly_wait를 사용해도 됩니다.

https://selenium-python.readthedocs.io/waits.html

```python
soup = BeautifulSoup(browser.page_source, 'html.parser')
locate = soup.select('#wob_loc')
date = soup.select('#wob_dts')
rain_percent = soup.select('#wob_pp')
```

page_source로 해당 페이지의 소스 코드를 가져와서 BeautifulSoup으로 파싱합니다.

각각 사용자의 위치와 날짜, 그리고 비 올 확률의 html 문자열을 저장하게 됩니다.

```python
for t in locate:
    print(t.text)

for t in date:
    print(t.text)

for t in rain_percent:
    print(t.text)
```

html 코드에서 텍스트만 추출합니다.

oo도 oo시 oo구 oo동
(o요일) 오후 10:00
54%

위와 같이 출력됩니다.

```python
browser.quit()
```

모든 작업이 완료되었으면 브라우저를 종료합니다.