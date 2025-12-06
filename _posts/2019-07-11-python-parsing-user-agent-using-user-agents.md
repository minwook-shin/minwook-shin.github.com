---
layout: post
title: Python 사용자 에이전트 파싱하는 user_agents 라이브러리 알아보기
---

오늘은 Python에서 브라우저, 운영체제의 정보를 알 수 있는 사용자 에이전트를 파싱할 수 있는 user_agents 라이브러리를 알아보려 합니다.

## user_agents 설치

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
pip install user-agents 
```

pip로 user-agents를 설치합니다.

## 기본 정보

```python
from user_agents import parse
```

user_agents에서 parse를 가져옵니다.

```python
ua_str = 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_5) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/12.1.1 ' \
         'Safari/605.1.15'
user_agent = parse(ua_str)
```

사용자 에이전트 문자열을 읽어서 파싱합니다.

```python
print(user_agent.browser, user_agent.browser.family, user_agent.browser.version, user_agent.browser.version_string)

print(user_agent.os, user_agent.os.family, user_agent.os.version, user_agent.os.version_string)

print(user_agent.device, user_agent.device.family, user_agent.device.brand, user_agent.device.model)
```

파싱한 사용자 에이전트로 브라우저, 운영제체, 장치에 대한 정보를 분리해서 알 수 있습니다.

```python
print(str(user_agent))
```

str로 출력할 수도 있습니다.

## 속성

```python
from user_agents import parse
```

user_agents에서 parse를 가져옵니다.

```python
ua_str = 'Mozilla/5.0 (Linux; U; Android 4.0.4; en-gb; GT-I9300 Build/IMM76D) AppleWebKit/534.30 (KHTML, ' \
         'like Gecko) Version/4.0 Mobile Safari/534.30 '
user_agent = parse(ua_str)
print(user_agent.is_mobile, user_agent.is_tablet, user_agent.is_pc)
print(user_agent.is_touch_capable)
print(user_agent.is_bot)
print(str(user_agent))
```

사용자 에이전트를 파싱하여 모바일인지, 태블릿인지 혹은 데스크톱인지 확인할 수 있으며, bot인지도 확인할 수 있습니다.

위 코드는 안드로이드 기기로서 터치가 가능한 모바일 장치로 인식합니다.

```python
ua_str = 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_5) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/12.1.1 ' \
         'Safari/605.1.15 '
user_agent = parse(ua_str)
print(user_agent.is_mobile, user_agent.is_tablet, user_agent.is_pc)
print(user_agent.is_touch_capable)
print(user_agent.is_bot)
print(str(user_agent))
```

사용자 에이전트를 파싱하여 모바일인지, 태블릿인지 혹은 데스크톱인지 확인할 수 있으며, bot인지도 확인할 수 있습니다.

위 코드는 맥북으로서 터치가 가능한 데스크톱으로 인식합니다.

```python
ua_str = 'Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; Trident/6.0; Touch)'
user_agent = parse(ua_str)
print(user_agent.is_mobile, user_agent.is_tablet, user_agent.is_pc)
print(user_agent.is_touch_capable)
print(user_agent.is_bot)
print(str(user_agent))
```

사용자 에이전트를 파싱하여 모바일인지, 태블릿인지 혹은 데스크톱인지 확인할 수 있으며, bot인지도 확인할 수 있습니다.

위 코드는 윈도우8 기기로서 터치가 가능한 데스크톱으로 인식합니다.