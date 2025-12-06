---
layout: post
title: VS Code 확장 만들고 배포하기
---

오늘은 자바스크립트로 VS Code 확장을 만들어보고 배포까지 해보겠습니다.

# 개요

거의 매일 vscode를 사용하면서 확장 프로그램은 어떻게 만들까 궁금해져서 간단한 토이 프로젝트를 만들어보았습니다.

공식 문서(https://code.visualstudio.com/api) 설명이 입문하기에 잘 작성되었으므로 해당 아티클에서는 가볍게 다뤄보려 합니다.

## 설정 및 기본 코드 준비

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install node
```

우선 맥으로 작업하는 경우에 Homebrew 설치하고, nodeJS 개발 환경을 만듭니다.

```bash
npm install -g yo generator-code
```

npm으로 yeoman generator 도구와 확장을 만들 수 있는 generator-code 플러그인을 설치합니다.

```
➜  ~ yo code                         

     _-----_     ╭──────────────────────────╮
    |       |    │   Welcome to the Visual  │
    |--(o)--|    │   Studio Code Extension  │
   `---------´   │        generator!        │
    ( _´U`_ )    ╰──────────────────────────╯
    /___A___\   /
     |  ~  |     
   __'.___.'__   
 ´   `  |° ´ Y ` 

? What type of extension do you want to create? (Use arrow keys)
❯ New Extension (TypeScript) 
  New Extension (JavaScript) 
  New Color Theme 
  New Language Support 
  New Code Snippets 
  New Keymap 
  New Extension Pack 
  New Language Pack (Localization) 
  New Web Extension (TypeScript) 
  New Notebook Renderer (TypeScript) 
```

yo code 명령어로 시작하면 어떤 확장 타입으로 시작할 것인지 물어봅니다.

대부분 New Extension으로 시작하고, 다른 선택지를 통해서 테마나 키맵도 커스텀할 수 있습니다.

```
? What type of extension do you want to create? New Extension (JavaScript)
? What's the name of your extension? test
? What's the identifier of your extension? test
? What's the description of your extension? 
? Enable JavaScript type checking in 'jsconfig.json'? No
? Initialize a git repository? (Y/n) 
? Which package manager to use? (Use arrow keys)
❯ npm 
  yarn 

...

Changes to package.json were detected.

Running npm install for you to install the required dependencies.

added 177 packages, and audited 178 packages in 14s

29 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities

Your extension test has been created!
```

확장의 이름, 구별자, 설명 등을 기입하면 하위 폴더로 새로운 프로젝트가 생성됩니다.

code 명령어로 프로젝트를 열면 준비가 끝납니다.

## 코딩

* package.json

```javascript
{
  "name": "test",
	"displayName": "test",
	"description": "",
	"version": "0.0.1",
	"engines": {
		"vscode": "^1.67.0"
	},
	"categories": [
		"Other"
	],
	"activationEvents": [
        "onCommand:test.helloWorld"
	],
	"main": "./extension.js",
	"contributes": {
		"commands": [{
            "command": "test.helloWorld",
            "title": "Hello World"

		}]
	}
    ...
}
```

프로젝트를 처음 열어보면 기입한 이름으로 Hello World 확장이 만들어진 상태입니다.

사용하려는 VSCODE API 버전에 맞춰서 vscode engine 버전을 맞추고, 카테고리는 아래 정해진 이름 중에 알맞는 것으로 변경해주면 됩니다.

```[Programming Languages, Snippets, Linters, Themes, Debuggers, Formatters, Keymaps, SCM Providers, Other, Extension Packs, Language Packs, Data Science, Machine Learning, Visualization, Notebooks, Education, Testing]```

명령을 커스텀하기 위해서 activationEvents, contributes 항목에서 바꿀 수 있습니다.

지금은 Hello World 명령이 수행되면 활성화되게 설정되어 있으나, 원하면 "onStartupFinished" 처럼 실행되며 활성화할 수도 있습니다.

https://code.visualstudio.com/api/references/activation-events

자세한 방식은 위 링크로 접근해서 확인할 수 있습니다.

* extension.js

```javascript
const vscode = require('vscode');

/**
 * @param {vscode.ExtensionContext} context
 */
function activate(context) {
	let disposable = vscode.commands.registerCommand('test.helloWorld', function () {
		vscode.window.showInformationMessage('Hello World from test!');
	});
	context.subscriptions.push(disposable);
}

function deactivate() {}

module.exports = {
	activate,
	deactivate
}
```

기존적으로 제공해주는 스크립트 파일입니다.

package.json 파일에서 등록한 test.helloWorld 명령을 수행하면 정보 메시지로 'Hello World from test!' 문구가 출력됩니다.

## 패키징

```
npm install -g vsce
```

VS Code Extension Manager 패키지를 전역 설치해서 패키징하고 배포해보려고 합니다.

```
vsce package
```

위와 같이 입력하면 확장을 패키징해서 로컬 VScode에 설치해볼 수 있습니다.

## 배포

패키지를 수동으로 설치해서 성공했다면, 이제 배포해보겠습니다.

```
vsce login ${publisher-id}
```

위와 같이 자신의 publisher를 입력하면 토큰으로 로그인할 수 있습니다.

토큰은 dev.azure.com 사이트에서 로그인한 뒤에 Personal Access Tokens 항목에서 만들 수 있습니다.

토큰 자체는 생성할 때만 노출되므로 복사 후 자신만 알 수 있는 공간에 잘 저장합니다.

```
vsce publish
```

자신의 계정으로 로그인에 성공하고 위와 같이 입력하면 확장을 마켓플레이스에 업로드할 수 있습니다.

업로드 후 약간의 시간이 지나면 아래와 같이 메일이 도착합니다.

```
EXTENSION VALIDATION SUCCESSFUL

Extension validation is complete for your extension. No issues were observed and the version is available for use in Visual Studio Marketplace.
```

이제 VS code 마켓플레이스에서 검색해서 설치할 수 있습니다.
