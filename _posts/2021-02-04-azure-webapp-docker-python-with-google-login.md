---
layout: post
title: Azure 웹앱으로 도커 파이썬 웹 사이트 만들어보고 구글 로그인 연동하기
---

오늘은 도커 컨테이너로 구현된 웹 서비스를 MS Azure 웹앱에 올려보고, 구글 로그인도 연동해보겠습니다.

## 준비물

* MS Azure account

Azure 웹앱을 사용하기 위해 필요합니다.

* Google account

구글 로그인을 위한 OAuth 설정을 위해 필요합니다.

* Docker image - web server (ex: nginx)

저는 이번 포스팅을 위헤서 docker.pkg.github.com 로 도커 이미지를 푸시해두었습니다.

## MS Azure 포털 로그인

우선 MS Azure 포털에 로그인합니다. 

https://portal.azure.com

## 웹앱 만들기

Azure 포털로 들어와서 App Services 에서 웹앱 만들기로 접근합니다.

### 리소스 그룹

기본탭에서 자신의 구독과 리소스 그룹을 확인하고, 그룹이 없다면 생성합니다.

이 때 자신의 구독의 크레딧을 확인해야 합니다.

### 인스턴스

인스턴스 이름을 만들어야 하는데, 해당 이름은 URL에 연동됩니다.

예를 들어서 test라는 인스턴스를 만들 경우에, test.azurewebsites.net URL로 결정됩니다.

### 게시

게시는 코드나 도커 컨테이너를 선택할 수 있으며, 여기에서는 Docker 컨테이너를 선택했습니다.

### 운영체제

운영 체제 윈도우와 리눅스를 선택할 수 있으며, 여기에서는 리눅스를 선택했습니다.

### 지역

지역 korea Central (한국 중부)를 선택했습니다. 중부가 서울이고 남부가 부산이라고 합니다.

### 요금제

App Service 요금제는 1GB 메모리를 가지고 있는 무료 F1을 사용합니다.

## Docker 

옵션에서 단일 컨테이너 및 도커 컴포즈를 선택할 수 있으며, 여기에서는 단일 컨테이너를 선택합니다.

도커 이미지 소스는 Azure Container Registry 이나 도커 허브 및 기타 프라이빗 레지스트리를 선택할 수 있지만, 해당 포스트에서 준비한 것은 github 패키지에 업로드한 도커 이미지이므로 프라이빗 레지스트리를 선택합니다.

자세한 설정은 아래를 참고해주시기 바랍니다.

* 서버 URL : https://docker.pkg.github.com

* 로그인 : 깃허브 사용자 이름

* 암호 : 깃허브에서 토큰 발급하기 

토큰은 Github Settings에서 발급 받습니다.

(Developer settings -> Personal access tokens)

write:packages scopes 가지면 됩니다.

* 전체 이미지 이름 및 태그 : docker.pkg.github.com/{사용자 이름}/repo/app:{versions}

## 로그

웹앱을 만들고, 배포 센터에서 컨테이너 로그를 확인할 수 있습니다.

### docker.pkg.github.com 패키지 오류

```
Image pull failed: Verify docker image configuration and credentials (if using private repository)
```

간혹 잘못된 값을 넣으면 위와 같이 이미지 PULL 안되는 현상이 있습니다.

주로 docker.pkg.github.com 에서 공개 이미지로 올렸다고 로그인 이름과 암호를 안적어도 될 것 같지만, 권한 이슈로 PULL을 받지 못할 수 있습니다.

깃허브에서 토큰을 발급해서 암호에 넣으면 다시 배포를 진행할 수 있습니다.

## Kudu 대시 보드

Kudu 대시 보드에서는 아래와 같은 URL로 접근할 수 있습니다.

https://{앱 서비스 이름}.scm.azurewebsites.net

Kudu 대시 보드에 접속하면 App Settings, Deployments, Source control info, Files, Docker logs를 볼 수 있으며 저와 같은 경우에는 로그 스트림을 볼 때 유용했습니다.

Kudu 대시 보드에서 /api/logstream로 접근하면 로그 스트림을 볼 수 있습니다.


## 구글 로그인 

추가로 인증/권한 부여에서 구글 로그인을 위한 클라이언트 ID 및 암호를 넣을 수 있습니다.

구글 로그인 OAuth2.0 API 발급을 위하여 구글 개발자 콘솔 https://console.developers.google.com/ 사이트에 접속하고, 새로운 프로젝트를 만듭니다.

API 및 서비스의 OAuth 동의 화면에서 앱 정보를 입력하고, 테스트 사용자에 본인의 이메일을 등록합니다.

다시 사용자 인증으로 넘어와서 사용자 인증 정보 만들기 버튼을 누르고, OAuth 2.0 클라이언트 ID를 선택합니다.

애플리케이션 유형을 선택하라고 하면 Azure 웹앱에 적용할 것이기 때문에 웹 애플리케이션을 선택합니다.

여기에서 리다렉션 URL을 https://{app}.azurewebsites.net/.auth/login/google/callback 으로 입력해줍니다.

만들어진 클라이언트 ID와 암호를 Azure의 인증/권한 부여 화면에서 "요청이 인증되지 않은 상황에서 Google로 로그인"하도록 선택하고, 미리 만들어둔 클라이언트 ID와 암호를 입력합니다.

이제 들어오는 토큰을 컨테이너로 이루어진 웹앱에서 사용하면 로그인된 정보를 이용할 수 있습니다.

이에 대한 자세한 내용은 ["Azure App Service의 고급 인증 및 권한 부여 사용"](https://docs.microsoft.com/ko-kr/azure/app-service/app-service-authentication-how-to) 공식 문서에서 찾아보실 수 있습니다.

여기까지 진행하면, Azure 웹앱 서비스로 웹 사이트를 도커 컨테이너 기반으로 만들어서 구글 로그인 연동까지 마무리할 수 있습니다.