---
layout: post
title: Python 웹 사이트 크롤링하는 Grab 라이브러리 알아보기
---

오늘은 Python에서 웹 사이트를 크롤링할 수 있는 Grab 라이브러리를 알아보려 합니다.

## Grab 설치

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
pip install -U Grab
```

pip로 Grab을 설치합니다.

```
pip uninstall pycurl
export PYCURL_SSL_LIBRARY=openssl
pip install pycurl
```

만약 openssl 관련 오류가 발생하면 해당 라이브러리의 의존성 패키지인 pycurl에서 문제가 발생할 것이므로 위와 같이 처리합니다.


## 예제

```python
from grab import Grab
```

Grab을 가져옵니다.

```python
g = Grab()
resp = g.go('http://example.com/')
```

Grab 객체로 example.com 사이트를 가져옵니다.

```python
print(resp.unicode_body())

print(resp.charset)

print(resp.body)
```

유니코드 문자열로 response body를 가져오거나 charset을 알아낼 수 있습니다.

```python
g = Grab()
g.go('http://testing-ground.scraping.pro/login')

g.doc.set_input('usr', 'admin')
g.doc.set_input('pwd', '12345')
g.submit()

print(g.doc.body)
```

Grab으로 입력을 넣어서 결과 페이지의 response body를 확인할 수 있습니다.

## spider 예제

```python
from grab.spider import Spider, Task
```

grab의 Spider를 가져옵니다.

```python
class Main(Spider):
    def task_generator(self):
        for lang in 'python', 'java', 'go':
            url = 'https://www.google.com/search?q=%s' % lang
            yield Task('result', url=url, lang=lang)
```

Task를 만들어 yield로 값을 발생시킵니다.

```python
    def task_result(self, grab, task):
        print('{}: {}'.format(task.lang, grab.doc('//*[@id="rhs_block"]').text()))
```

result라는 Task이므로 task_result 함수를 만들고, 해당 task가 수행될 때에 출력할 문구를 작성합니다.

해당 선택자에 포함되는 문자열이 출력됩니다.

```python
bot = Main(thread_number=3)
bot.run()
```

위 클래스로 객체를 만들어서 실행합니다.