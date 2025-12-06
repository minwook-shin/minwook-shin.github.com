---
layout: post
title: 구글 크롬 확장 프로그램 만들기
---

오늘은 구글 크롬의 확장 프로그램을 만들려고 입문해보시는 분을 대상으로 간단하게 알려드리는 정도의 포스팅을 하려고 합니다.

사실은 제가 약 1년 전에 만들어둔 구글 크롬 확장 프로그램 소스코드를 다시 보자니, 슬슬 확장 프로그램을 만드는 법을 까먹어가려고 하는 것 같아서 블로그에 기록해두려 하는 목적도 있습니다.
ㅎㅎ

## 정의 

[위키백과](https://ko.wikipedia.org/wiki/구글_크롬_확장_프로그램)에 따르면,

```
구글 크롬 확장 프로그램(Google Chrome Extension)은 구글 크롬 브라우저를 수정하는 브라우저 확장 프로그램이다. 이러한 확장은 HTML, JavaScript 및 CSS 와 같은 웹 기술을 기반으로 사용하여 작성된다.
```

라고 언급이 되어 있으며, 이 설명처럼 웹 기술로 모든 게 동작하게 됩니다.

## 필요한 파일 설명

* manifest.json : 확장 프로그램의 이름과 버전과 같은 정보를 기재하는 파일입니다.
확장 프로그램을 구성할 html 페이지나 이미지, 권한 등을 조정하게 됩니다.

* icon 파일 : 확장 프로그램의 로고를 등록합니다. manifest.json에서 ``` "icons":```으로 등록할 수 있습니다.
해상도별로 나누어서 관리할 수도 있습니다.

* html 파일  : 확장 프로그램의 메인 부분을 작성하는 파일입니다. 

```json
"browser_action": {
        "default_popup":
```

manifest.json에서 위와 같이 등록할 수 있습니다.

* js 파일 : 동적 기능이 필요할 때 자바스크립트 파일을 작성합니다. 되도록 CDN보다는 로컬에서 불러오는 게 정신건강에 이로우므로, 나중에 가면 모든 기능을 JS파일에 쌓아놓는 자신을 발견하게 되는 파일입니다.
이 역시, 아래에서 설명하겠지만 manifest.json에서 "permissions"과 "content_scripts","background"를 잘 이용해야 합니다. (기본적으로 BASIC한 확장 프로그램에서는 앱이 가진 권한은 거의 없습니다.)



## 확장 프로그램 제작

1. 구글 크롬을 설치합니다.

2. manifest.json 파일을 생성합니다.

[구글 크롬 개발자 사이트](https://developer.chrome.com/extensions/manifest)를 들어가  보시면 아래와 같이 나와 있습니다.

```json
{
  "manifest_version": 2,
  "name": "My Extension",
  "version": "versionString",
```
위는 필수적으로 요구되는 항목입니다.

확장 프로그램의 이름과 버전을 기입합니다.

이는 나중에 크롬 웹스토어에 업로드할 때로 표시됩니다.

```json
  "default_locale": "en",
  "description": "A plain text description",
  "icons": {...},
```

권장되는 항목입니다.

기본 언어와 해당 프로그램의 설명을 기입하고 아이콘도 기본에서 수정할 수 있습니다.

```json
  // Pick one (or none)
  "browser_action": {...},
```

브라우저의 행동, 즉 ```"default_popup":```와 같이 기본적으로 출력할 파일을 지정합니다.

```json
  "action": ...,
  "author": ...,
  "automation": ...,
  "background": {
    // Recommended
    "persistent": false
  },
  "background_page": ...,
  "chrome_settings_overrides": {...},
  "chrome_ui_overrides": {
    "bookmarks_ui": {
      "remove_bookmark_shortcut": true,
      "remove_button": true
    }
  },
  "chrome_url_overrides": {...},
  "commands": {...},
  "content_capabilities": ...,
  "content_scripts": [{...}],
  "content_security_policy": "policyString",
  "converted_from_user_script": ...,
  "current_locale": ...,
  "declarative_net_request": ...,
  "devtools_page": "devtools.html",
  "event_rules": [{...}],
  "externally_connectable": {
    "matches": ["*://*.example.com/*"]
  },
  "file_browser_handlers": [...],
  "file_system_provider_capabilities": {
    "configurable": true,
    "multiple_mounts": true,
    "source": "network"
  },
  "homepage_url": "http://path/to/homepage",
  "import": [{"id": "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"}],
  "incognito": "spanning, split, or not_allowed",
  "input_components": ...,
  "key": "publicKey",
  "minimum_chrome_version": "versionString",
  "nacl_modules": [...],
  "oauth2": ...,
  "offline_enabled": true,
  "omnibox": {
    "keyword": "aString"
  },
  "optional_permissions": ["tabs"],
  "options_page": "options.html",
  "options_ui": {
    "chrome_style": true,
    "page": "options.html"
  },
  "permissions": ["tabs"],
  "platforms": ...,
  "requirements": {...},
  "sandbox": [...],
  "short_name": "Short Name",
  "signature": ...,
  "spellcheck": ...,
  "storage": {
    "managed_schema": "schema.json"
  },
  "system_indicator": ...,
  "tts_engine": {...},
  "update_url": "http://path/to/updateInfo.xml",
  "version_name": "aString",
  "web_accessible_resources": [...]
}
```

여기까지는 부분적으로 원하는 것만 분리해서 적용해도 됩니다. 모든 것을 설명해드리지 못하지만, [해당 링크](https://developer.chrome.com/extensions/manifest)에 자세한 설명이 첨부되어 있으니 확인바랍니다.

저와 같은 경우는 아래처럼 구성하여 이용하고 있습니다.

(참고로 ```__MSG_extension_desc__```를 사용하여 다국어 지원도 됩니다.) 

```json
{
    "author": "minwook-shin",
    "manifest_version": 2,
    "default_locale": "ko",
    "name": "__MSG_extension_name__",
    "short_name": "__MSG_extension_sname__",
    "description": "__MSG_extension_desc__",
    "version": "0.0.1",
    "version_name": "preview",

    "browser_action": {
        "default_icon": "symbol_color.png",
        "default_popup": "popup.html"
    },

    "icons": {
        "128": "symbol_color.png"
    },

    "options_ui": {
        "chrome_style": true,
        "page": "options.html"
    },

    "permissions": [
        "activeTab", "http://*/*", "https://*/*"
    ],

    "content_scripts": [{
        "matches": ["file://home.html"],
        "js": ["scripts.js"],
        "css": ["css.css"]
    }],

    "background": {
        "scripts": ["background.js"],
    }
}
```

3. 출력할 html 코드를 작성합니다.

4. html에 작동하는 기능에 맞게 (제 예시처럼) manifest.json에서 "permissions"과 "content_scripts","background"를 알맞게 추가해줍니다.

5. 파일들을(json,html,js...) 전부 한 폴더에 넣어줍니다.

6. 구글 크롬의 주소창에 chrome://extensions로 이동합니다.

7. 개발자 모드에 체크합니다.

8. 압축 해제된 확장 프로그램 로드... 라는 버튼으로 만든 프로그램의 폴더를 열어줍니다.

9. 옵션) 크롬에서 잘 작동되는 것을 확인하면 [크롬 웹 스토어](https://chrome.google.com/webstore/category/extensions) 에 개발자 등록을 하고 앱을 업로드합니다.

## 뭔가 나만 오류가 난다면?

구글의 샘플 코드는 작동하는데 자신의 코드가 작동을 안 하는 거의 모든 경우가 권한, 즉 permissions에 문제가 발생합니다.

[크롬 개발자 사이트의 declare_permissions 문서](https://developer.chrome.com/extensions/declare_permissions)에서 보시면, 엑티브 탭과 백그라운드와 같이 앱의 동작은 물론이고 기록, 알림, 프린터 출력과 같이 크롬의 정보를 공유하는 모든 것이 권한을 요구합니다.

그래서 어떤 기능을 쓰고 싶다거나 적용했는데 작동을 안 한다면, 구글링으로 permissions을 검색해보시면 웬만한 문제점들은 해결됩니다.

## 번외) hello, world! 를 띄우자!

이 부분은 크롬 개발자 사이트에 언급되어있는 내용이지만, 위 내용만 봐서 크롬 확장 프로그램에 대해 감이 안 오시는 분들들 위해 번외편으로 따라가기를 준비해보았습니다.

우선 manifest.json 파일을 만들고 아래 내용을 작성합니다.

이름과 설명, 버전은 마음대로 작명해도 되지만, "manifest_version"은 manifest 파일 형식의 버전을 지정하는 하나의 정수이므로 무조건 2로 해두셔야 합니다.

```json
  {
    "name": "Hello Extensions",
    "description" : "Base Level Extension",
    "version": "1.0",
    "manifest_version": 2
  }
  ```

그리고 manifest 파일만으로는 무언가를 출력할 수 없으므로, 우리 눈에 보이는 html파일을 작성합니다.

```html
<html>
    <body>
        <h1>Hello World!</h1>
    </body>
</html>
```

그리고 다시 manifest 파일로 돌아와서 아래 코드를 추가합니다.

위 html 파일과 연결하고 엑티브탭에 대한 권한을 주는 역할을 하게 됩니다.

```json
"browser_action": {
      "default_popup": "index.html"
    }
"permissions": [
        "activeTab", "http://*/*", "https://*/*"
    ],
```


구글 크롬의 주소창에 chrome://extensions로 이동하여, 개발자모드에 체크합니다.

압축해제된 확장 프로그램 로드... 라는 버튼으로 만든 프로그램의 폴더를 열어줍니다.

## 구글에서 만든 샘플을 실행해보기

<https://developer.chrome.com/extensions/samples>에서 다양한 예제를 제공해주며, API별로 선택하거나, 키워드 검색을 해서 원하는 방향의 예제를 볼 수 있습니다.

우리가 위에서 hello, world 번외편과 매우 흡사한 구글의 예제는 [이 곳](https://developer.chrome.com/extensions/examples/tutorials/hello_extensions.zip)에서 내려받을 수 있습니다.