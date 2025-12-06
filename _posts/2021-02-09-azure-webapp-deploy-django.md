---
layout: post
title: VS code로 Azure 웹앱에 Django 3.1 웹사이트 배포하기
---

오늘은 VS code로 Django 3.1 웹 사이트를 MS Azure 웹앱에 배포해보겠습니다.

## 준비물

* MS Azure account

Azure 웹앱을 사용하기 위해 필요합니다.

* python 3.6 이후 버전

Django 3.1 버전부터 python 3.6 이후 버전을 지원합니다.

## MS Azure 포털 로그인

우선 MS Azure 포털에 로그인합니다.

https://portal.azure.com

## 웹앱 만들기

Azure 포털로 들어와서 App Services 에서 웹앱 만들기로 접근합니다.

(ms-vscode.vscode-node-azure-pack 설치하여 Azure 계정을 로그인하면, vscode 내부에서 웹 앱을 생성할 수도 있습니다.)

### 리소스 그룹

기본 탭에서 자신의 구독과 리소스 그룹을 확인하고, 그룹이 없다면 생성합니다.

이 때 자신의 구독의 크레딧을 확인해야 합니다.

### 인스턴스

인스턴스 이름을 만들어야 하는데, 해당 이름은 URL에 연동됩니다.

예를 들어서 test라는 인스턴스를 만들 경우에, test.azurewebsites.net URL로 결정됩니다.

### 게시

게시는 코드나 도커 컨테이너를 선택할 수 있으며, 여기에서는 코드를 게시하도록 선택했습니다.

### 런타임 스택

런타임 스택에서는 다양한 언어를 선택할 수 있으며, 여기에서는 파이썬 3.8을 선택했습니다.

### 지역

지역 korea Central (한국 중부)를 선택했습니다. 중부가 서울이고 남부가 부산이라고 합니다.

### 요금제

App Service 요금제는 1GB 메모리를 가지고 있는 무료 F1을 사용합니다.

## VS code Azure Tools 확장

ms-vscode.vscode-node-azure-pack 확장을 설치합니다.

Extension Pack이라서 11개의 확장이 같이 설치됩니다.

설치가 끝나면, 왼쪽에 Azure 아이콘이 나타납니다.

Azure 아이콘을 처음 누르게 된다면, 명령 팔레트 창에서 "Azure: Sign in" 으로 로그인 합니다.

로그인을 완료하면, "You are signed in now and can close this page"라는 창이 나오고 VS code로 돌아옵니다.

다시 Azure 아이콘을 누르면 자신의 구독별 리소스 그룹, 가상머신, 앱 서비스, 함수, 데이터베이스, 스토리지를 볼 수 있으며 오늘은 앱 서비스를 누릅니다.

이제 앱 서비스를 만들어두었으니, 배포를 위한 Django 웹 사이트도 만들겠습니다.

## Django 사이트 만들기

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
pip install Django
```

pip로 Django 라이브러리를 설치합니다.

django-admin으로 원하는 이름으로 django 프로젝트를 만듭니다.

django-admin startproject {project_name}

만들어진 django 프로젝트 폴더로 진입하고, 처음 서버를 구동하면 로컬호스트 8000 포트로 들어갈 수 있습니다.

cd {app_name}
python manage.py runserver 8000

```markdown
Django version 3.1, using settings '***.settings'
Starting development server at http://0:8000/
Quit the server with CONTROL-C.
```

프로젝트 내부에서 django 앱을 만들어줍니다.

python manage.py startapp {app}

만들었던 앱 폴더에서 views.py 와 urls.py 파일에 앱 index 관련 코드를 작성하고, 프로젝트 폴더의 urls.py 파일로 앱의 URL을 연결합니다.

지금은 별도의 데이터베이스를 연결하지 않고 SQLite로만 할 예정이므로, settings.py 파일의 INSTALLED_APPS에 적힌 앱에서 데이터베이스를 사용할 수 있도록 migrate 명령을 입력합니다.

python manage.py migrate

```markdown
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying auth.0010_alter_group_name_max_length... OK
  Applying auth.0011_update_proxy_permissions... OK
  Applying auth.0012_alter_user_first_name_max_length... OK
  Applying sessions.0001_initial... OK
```

models.py 파일에서 데이터베이스 모델을 정의하고, migrate 명령으로 인식하기 위하여 makemigrations 명령을 입력합니다.

python manage.py makemigrations {app}

```markdown
Migrations for '{app}':
  {app}/migrations/0001_initial.py
    - Create model {model name}
```

앱에서 데이터베이스를 사용할 수 있도록 migrate 명령을 입력합니다.

python manage.py migrate

```markdown
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, {app}, sessions
Running migrations:
  Applying {app}.0001_initial... OK
```

이제 어드민 페이지에 들어갈 수 있도록 관리자 계정을 만듭니다.

python manage.py createsuperuser

```markdown
Username (leave blank to use '***'): 
Email address: ***@gmail.com
Password: 
Password (again): 
Superuser created successfully.
```

이제 다시 서버를 열면 만든 앱의 페이지와 모델이 어드민 페이지에서 보이게 됩니다.

python manage.py runserver

그리고 settings.py 파일의 ALLOWED_HOSTS 배열에 도메인을 추가해야 프로덕션 환경에서 사용할 수 있습니다.

추가로 Azure Storage로 연결하면 관련 데이터도 저장할 수 있습니다.

## 배포하기

Azure CLI를 이용하거나 VS code의 확장으로 배포할 수 있습니다.

Azure CLI는 아래 주소로 자세히 알 수 있으며, 해당 포스트에서는 VS code 확장으로 진행합니다.

https://docs.microsoft.com/en-us/cli/azure/install-azure-cli

VS code Azure Tools 확장을 설치했다면, 에디터의 왼쪽에 Azure 아이콘이 나타납니다.

앱 서비스로 접근하여, 이미 만들어둔 앱 서비스에서 우측 클릭을 하여 "Deploy to Web App" 버튼을 누르거나 앱 서비스 우측에 있는 버튼으로 배포를 진행합니다.

배포할 django 앱 폴더를 선택하고, 빌드 구성 업데이트와 기존 배포 덮어쓰는 여부를 묻는 메시지가 나오면 동의합니다.

처음에 배포하면 django를 인식하지만, gunicorn 등 사용하려면 Azure 포털의 웹앱 설정에서 시작 명령어를 지정해주어야 합니다.

## 로그

웹앱을 만들고, 배포 센터에서 컨테이너 로그를 확인할 수 있습니다.

또는 VS code에서도 Azure 확장을 통해 원하는 웹앱의 로그 스트리밍을 터미널로 보면서 진행할 수 있습니다.

## Kudu 대시 보드

Kudu 대시 보드에서는 아래와 같은 URL로 접근할 수 있습니다.

https://{앱 서비스 이름}.scm.azurewebsites.net

Kudu 대시 보드에 접속하면 App Settings, Deployments, Source control info, Files, Docker logs를 볼 수 있습니다.
