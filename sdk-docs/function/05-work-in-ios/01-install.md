---
sidebar_label: 설치하기
description: "설치하기"
---

# 뒤끝펑션 설치 For Mac

뒤끝펑션을 개발하기 위해서는 **개발 툴과 Visual Studio Code Extension을 반드시 설치**해야 합니다.  
개발사에서는 Visual Studio Code Extension을 이용하여 Visual Studio Code 환경에서 새로운 프로젝트를 생성할 수 있습니다.  
개발사에서는 뒤끝펑션 개발 툴을 이용하여 위의 템플릿을 이용하여 생성한 프로젝트의 디버깅 및 빌드, 서버로 배포, 배포된 프로젝트의 삭제를 진행할 수 있습니다.  

- [뒤끝펑션 개발 툴 for Mac-arm64](https://developer.thebackend.io/sdk/BackendFunction/1.0.2/SetupForMac-arm64.pkg) \[2025-08-08]
- [뒤끝펑션 개발 툴 for Mac-x64](https://developer.thebackend.io/sdk/BackendFunction/1.0.2/SetupForMac-x64.pkg) \[2025-08-08]
- [뒤끝펑션 프로젝트 템플릿 for VSC](https://marketplace.visualstudio.com/items?itemName=AFI.backendfunction) \[2025-08-08]
  > 뒤끝펑션 프로젝트는 **개발 툴에 포함된 디버거 프로그램 및 빌드 플러그인**을 통해서만 디버깅 및 빌드/배포가 가능합니다.  

### 뒤끝펑션 개발 툴 설치하기

위 링크로 개발 툴을 다운로드한 후 **SetupForMac** 패키지 파일을 실행하면 설치가 진행됩니다.  

다운로드 및 실행시 다음과 같은 알림이 발생할 수 있습니다.  
해당 모달이 발생할 경우 열기를 눌러 프로그램을 열어주시기 바랍니다.  

<img src="https://developer.thebackend.io/static/img/bfunc/osx/install/error-1.png" />

다운로드가 불가하거나 실행이 안될 경우에는 응용프로그램 **시스템 환경설정 - 보안 및 개인정보 보호**에 들어가 'SetupForMac' 패키지 좌측 버튼 **확인 없이 열기**를 클릭합니다. 해당 작업은 웹에서 다운받을 때와 프로그램을 실행시킬 때 총 2번 발생할 수 있습니다.  

<img src="https://developer.thebackend.io/static/img/bfunc/osx/install/error-2.png" />

설치 프로그램을 실행하여 절차에 따라 진행해 주시기 바랍니다.  

- 프로그램 경로는 변경되어서는 안 됩니다.  
- 설치되는 경로는 "/Applications/AFI/BFunc/"입니다

![image](/img/docs/guide/function/setting/mac/install-mac-1.png)  
![image](/img/docs/guide/function/setting/mac/install-mac-2.png)  
![image](/img/docs/guide/function/setting/mac/install-mac-3.png)  

**설치 완료 후 반드시 컴퓨터를 재시동**해야 설치가 정상적으로 완료됩니다.  

### VSCode 용 뒤끝펑션 확장 설치하기

:::info 뒤끝펑션은 Visual Studio For Mac을 지원하지 않습니다.  
Mac 환경에서는 Visual Sutdio Code에서 제공하는 Extensions(vsix)만 지원하고 있으며, 팔레트 명령어를 통해 템플릿을 이용하실 수 있습니다.  
:::

Visual Studio Code 내 확장에서 검색을 이용해 BackendFunction을 검색 후, BackendFunction을 찾아 설치합니다.  
또는 [웹페이지 Vscode 내 MarketPlace](https://marketplace.visualstudio.com/items?itemName=AFI.backendfunction)에서 Install 버튼을 눌러 설치합니다.  

![image](/img/docs/guide/function/intro/install-extension.png)

## 뒤끝펑션 제거

### 프로그램

뒤끝펑션을 제거하려면 Applications(응용 프로그램)에 있는 AFI 폴더를 제거해야 합니다.  
Applications/AFI 폴더를 우 클릭하여 '휴지통으로 이동'을 클릭하거나 드래그하여 휴지통으로 이동하여 삭제하실 수 있습니다.  

<img src="https://developer.thebackend.io/static/img/bfunc/osx/install/deletePath-2.png" />

### 환경 변수

등록된 뒤끝펑션에 대한 환경 변수의 설정을 제거하려면 아래의 응용 프로그램을 다운로드 및 실행하여 제거하실 수 있습니다.  

- [뒤끝펑션 환경 변수 제거 프로그램](https://developer.thebackend.io/sdk/BackendFunction/0.3.0/uninstallForMac) \[2023-08-29]
  uninstallForMac.sh

<img src="https://developer.thebackend.io/static/img/bfunc/osx/install/deletePath-1.png" />
