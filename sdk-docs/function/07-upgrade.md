---
sidebar_label: 펑션 SDK 업그레이드
description: "펑션 SDK 업그레이드"
---

# 펑션 SDK 업그레이드

## 펑션 SDK 업그레이드 방법

펑션 SDK는 개발 툴에 첨부된 API를 복사하여 현재 사용중인 프로젝트에 붙여넣는 형식으로 업데이트를 진행해야합니다.  
해당 펑션 SDK 업그레이드 가이드 문서는 **뒤끝 펑션 버전 1.0.0** 기준으로 작성 되었습니다.  

### 1.최신 버전의 펑션 다운로드

[펑션 시작하기](/sdk-docs/function/intro)에서 새로 배포된 개발툴을 다운 받고 설치를 진행합니다.  

<img src="/img/docs/guide/function/migrate/new-install-finish.png" />

### 2. 펑션이 설치된 폴더로 이동

이후 펑션이 설치된 폴더로 이동합니다.  

- Windows : C:\Program Files\AFI\BackendFunction\API
- Mac : Applications(응용프로그램)\AFI\BFunc\API

**Window 경로**  

<img src="/img/docs/guide/function/upgrade/new-window-install.png" />  

**Mac 경로**  

<img src="https://developer.thebackend.io/static/img/bfunc/upgrade/mac-install.png" />

### 3. API 폴더 내 있는 dll 복사

API 폴더에 있는 dll 파일들을 전부 복사합니다.  

<img src="/img/docs/guide/function/upgrade/new-window-copy.png" />  

### 4. 사용중인 뒤끝펑션 프로젝트에 붙여넣기

업그레이드할 펑션의 프로젝트 > dll 폴더에 복사한 파일들을 붙여넣습니다.  

<img src="/img/docs/guide/function/upgrade/new-window-paste.png" />  

### 5. 기존 BackendAPI.dll 파일 삭제하기

파일을 붙여넣기한 dll 폴더에 남아있는 BackendAPI.dll파일을 삭제합니다.   

<img src="/img/docs/guide/function/upgrade/new-window-delete.png" />  

### 6. 프로젝트 속성 수정하기

#### Visual Studio의 경우  

1. 솔루션 탐색기에서 BackendFunction을 더블 클릭하여 열리는 csproj 파일의 BackendAPI 부분을 찾아 확인합니다.  

<img src="/img/docs/guide/function/upgrade/new-window-modify-csproj-vs-step1.png" />  


2. BackendAPI부분을 Backend로 수정하고 저장합니다.  

<img src="/img/docs/guide/function/upgrade/new-window-modify-csproj-vs-step2.png" />  

#### VSCode의 경우  

1. 탐색기에서 BackendFunction.csproj 파일을 열여 BackendAPI 부분을 찾습니다.  

<img src="/img/docs/guide/function/upgrade/new-window-modify-csproj-vscode-step1.png" />  


2. BackendAPI부분을 Backend로 수정하고 저장합니다.  

<img src="/img/docs/guide/function/upgrade/new-window-modify-csproj-vscode-step2.png" />  

### 7. 펑션 템플릿 수정(선택 사항)

펑션 디버거가 비동기를 환경에서 테스트가 가능해 졌습니다.  
개발 상황에 따라 동기/비동기 방식을 선택하여 구현 하실 수 있습니다.

:::caution 펑션 비동기 처리 이슈
펑션을 비동기로 구현하여 처리 할 경우 동기 방식 보다 응답 속도가 떨어지는 현상이 발생 할 수 있습니다.  
우선적으로 동기 방식으로 구현하는 것을 권장드리며 비동기 처리가 필요한 상황에 따라서 선택적으로 사용해 주세요.
:::

1.기존 Function함수(동기 방식)
<img src="/img/docs/guide/function/upgrade/new-template-old.png" />  


2.비동기 Function함수
<img src="/img/docs/guide/function/upgrade/new-template-async.png" />  

### 8. 빌드 후 배포

이후 빌드 및 배포하여 새로운 sdk의 펑션을 업로드합니다.  

<img src="https://developer.thebackend.io/static/img/bfunc/upgrade/window-deploy.png" />

에러가 발생할 경우에는 .NET 8.0 버전이 설치되어 있는지, Backend.dll의 경로나 파일명이 정확하게 되어있는지 확인합니다.
