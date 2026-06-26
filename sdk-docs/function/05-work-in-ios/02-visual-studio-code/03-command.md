---
sidebar_label: 명령어 모음
---

# VSCode 뒤끝펑션용 명령 팔레트

개발 툴로 Visual Studio Code를 사용할 경우에는 BackendFunctionExtension을 설치한 후,
명령 팔레트(단축키 F1, 또는 Ctrl + Shift + P)를 이용하여 설정, 빌드, 배포 관련 명령어를 사용할 수 있습니다.  

> 정상적인 작동을 위해서는 뒤끝펑션 개발 툴이 설치되어 있어야 합니다.  
> 프로젝트 경로에 띄어쓰기가 포함될 경우, 명령어들이 정상적으로 작동하지 않을 수 있습니다.  

사용 가능한 명령어는 다음과 같습니다.  

| 명령어                                                  | 설명                                                                              |
| ------------------------------------------------------- | --------------------------------------------------------------------------------- |
| BackendFunction: Create                                 | 폴더를 지정하여 새로운 폴더를 만들고 내부에 템플릿을 설치합니다                   |
| BackendFunction: Config                                 | 빌드 및 배포에 사용되는 config.json을 엽니다.                                     |
| BackendFunction: Build Current Project                  | 현재 프로젝트의 BackendFunction.csproj를 이용하여 빌드합니다.                     |
| BackendFunction: Build by config.json                   | config.json에 저장된 정보를 이용하여 빌드합니다.                                  |
| BackendFunction: Deploy                                 | 빌드 한 파일(publish.zip)를 지정하고 등록될 펑션 이름을 직접 설정하여 배포합니다. |
| BackendFunction: Deploy by config.json with FuntionName | config.json에 등록된 프로젝트 정보와 등록될 펑션이름을 직접 설정하여 배포합니다.  |
| BackendFunction: Deploy by config.json                  | config.json에 등록된 정보를 이용하여 배포합니다.                                  |
| BackendFunction: Delete                                 | 함수명을 입력하여 뒤끝에 배포된 함수를 제거합니다.                                |

## 명령 호출

1.  뒤끝 개발 툴과 vscode 내 마켓 플레이스에서 BackendFunction 확장 프로그램을 설치합니다.  
2.  F1 또는 Ctrl + Shift + P를 눌러 명령 팔레트를 엽니다.  
3.  명령 팔레트에 명령어를 입력합니다.  
4.  이후 vscode 내에 통합 터미널을 통해 결과를 확인합니다.  

![image](https://developer.thebackend.io/static/img/bfunc/vscodecommand/1.PNG)

## 생성

### >BackendFunction: Create

폴더를 지정하여 새로운 폴더를 만들고 해당 폴더 내부에 템플릿을 설치합니다.  
이후 해당 프로젝트로 새로운 vscode 창이 열립니다.  

## Config.Json 설정

### BackendFunction: Config

빌드 및 배포에 사용되는 config.json을 엽니다.  

![image](https://developer.thebackend.io/static/img/bfunc/vscodecommand/Config-1.PNG)

## 빌드

### >BackendFunction: Build Current Project

현재 프로젝트의 BackendFunction.csproj를 이용하여 빌드합니다.  
빌드 된 프로젝트는 작업 폴더/bin/Release/net8.0/linux-x64/ 폴더 내, publish.zip로 생성됩니다.  
이후, 해당 zip을 지정하여 배포할 수 있습니다.  

![image](https://developer.thebackend.io/static/img/bfunc/vscodecommand/Build-1.PNG)

### >BackendFunction: Build by config.json

config.json에 저장된 정보를 이용하여 빌드합니다.  
명령어 BakcnedFunction: Config를 통해 빌드에 사용될 config.json을 수정할 수 있습니다.  

![image](https://developer.thebackend.io/static/img/bfunc/vscodecommand/Build-2.PNG)

## 배포

### >BackendFunction: Deploy

빌드 한 파일(publish.zip)를 지정하고 뒤끝에 등록될 펑션 이름을 직접 설정하여 배포합니다.  

---
####
1. 빌드 한 파일을 지정합니다. 대부분의 경로는 /bin/Release/net8.0/linux-x64/publish.zip입니다.  

![image](https://developer.thebackend.io/static/img/bfunc/osx/vscode/vscodecommand/mac-deploy-1.png)

---
####
2. 입력창에 뒤끝에 등록할 FunctionName을 입력합니다.  

![image](https://developer.thebackend.io/static/img/bfunc/vscodecommand/Deploy1-2.PNG)

---
####
3. 통합 터미널에서 배포할 정보를 확인하고 y/n을 눌러 진행합니다.  

![image](https://developer.thebackend.io/static/img/bfunc/vscodecommand/Deploy1-3.PNG)

### >BackendFunction: Deploy by config.json with FuntionName

뒤끝에 등록될 펑션 이름을 직접 설정하여 배포합니다. 빌드 파일은 config.json에서 설정한 경로에 있는 파일로 배포합니다

---
####
1. 입력창에 뒤끝에 등록할 FunctionName을 입력합니다.  

![image](https://developer.thebackend.io/static/img/bfunc/vscodecommand/Deploy1-2.PNG)

---
####
2. 통합 터미널에서 배포할 정보를 확인하고 y/n을 눌러 진행합니다.  

![image](https://developer.thebackend.io/static/img/bfunc/vscodecommand/Deploy1-3.PNG)

### >BackendFunction: Deploy by config.json

config.json에 저장된 정보를 이용하여 배포합니다.  
명령어 BakcnedFunction: Config를 통해 빌드에 사용될 config.json을 수정할 수 있습니다.  

---
####
1. 통합 터미널에서 배포할 정보를 확인하고 y/n을 눌러 진행합니다.  

![image](https://developer.thebackend.io/static/img/bfunc/vscodecommand/Deploy1-3.PNG)

## 삭제

### >BackendFunction: Delete

함수명을 입력하여 뒤끝에 배포된 함수를 제거합니다.  

![image](https://developer.thebackend.io/static/img/bfunc/vscodecommand/Deploy1-2.PNG)

![image](https://developer.thebackend.io/static/img/bfunc/vscodecommand/Delete-1.PNG)
