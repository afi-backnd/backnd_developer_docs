---
sidebar_label: 시작하기
description: "시작하기"
---

# 시작하기

뒤끝펑션은 개발사에서 직접 서버에 호출할 함수를 개발 및 사용하기 위한 기능입니다.  
뒤끝펑션에서 제공하는 템플릿을 활용하면 손쉽게 서버에서 동작할 기능을 개발할 수 있습니다.  

뒤끝베이스와 별개로 뒤끝펑션만 사용할 수 없습니다.  
뒤끝베이스에서 배포된 뒤끝펑션을 호출하는 방법은 [뒤끝펑션](/sdk-docs/backend/base/function/execute-function) 문서를 참고해 주세요.  

## 뒤끝펑션 인증키 발급

뒤끝펑션을 사용하기 위해서는 뒤끝콘솔에서 먼저 활성화를 해야 합니다.  
뒤끝콘솔에서 뒤끝펑션 메뉴를 선택한 뒤 활성화를 하면 인증키를 발급받을 수 있습니다.  

![](/img/docs/guide/function/intro/function-key.png)

해당 인증키는 유니티에서 뒤끝 SDK를 임포트한 이후, 상단의 The Backend > Edit Settings에서 Function Auth Key에 입력해야 합니다.

![](/img/docs/guide/function/intro/function-unity.png)
 

## 뒤끝펑션 개발 환경

뒤끝펑션을 개발하기 위해서는 아래 개발 환경이 필요합니다.  
* 윈도우 10 이상
* Visual Studio 2022 혹은 Visual Studio Code
* [.NET 8.0](https://dotnet.microsoft.com/download/dotnet/8.0) 
<div id = "download"></div> 

## .NET 8.0 설치
뒤끝펑션은 .NET 8.0으로 이루어져 있으며, 테스트 및 펑션 업로드를 하기 위해서는 .NET 8.0.x 버전이 필요합니다.  

[.NET 8.0 다운로드](https://dotnet.microsoft.com/download/dotnet/8.0)

## 뒤끝펑션 설치

뒤끝펑션을 개발하기 위해서는 **개발 툴과 Visual Studio 프로젝트 템플릿을 반드시 설치**해야 합니다.  
개발사에서는 프로젝트 템플릿을 이용하여 Visual Studio 환경에서 새로운 프로젝트를 생성할 수 있습니다.  
또한 개발 환경이 Visual Studio Code일 경우에는 마켓플레이스에서 **확장 BackendFunction**을 설치하여 이용하실 수 있습니다.  
개발사에서는 뒤끝펑션 개발 툴을 이용하여 위의 템플릿을 이용하여 생성한 프로젝트의 디버깅 및 빌드, 서버로 배포, 배포된 프로젝트의 삭제를 진행할 수 있습니다.  

* [뒤끝펑션 개발 툴 for Windows](https://developer.thebackend.io/sdk/BackendFunction/1.0.2/Setup.exe) \[2025-08-08]
* [뒤끝펑션 프로젝트 템플릿 for VS](https://developer.thebackend.io/sdk/BackendFunction/1.0.2/BackendFunctionVSIX.vsix) \[2025-08-08]
* [뒤끝펑션 프로젝트 템플릿 for VSC](https://marketplace.visualstudio.com/items?itemName=AFI.backendfunction) \[2025-08-08]
> 뒤끝펑션 프로젝트는 **개발 툴에 포함된 디버거 프로그램 및 빌드 플러그인**을 통해서만 디버깅 및 빌드/배포가 가능합니다.  



### 뒤끝펑션 개발 툴 설치하기

위 링크로 개발 툴을 다운로드한 후 BackendSetup.msi 파일을 실행하면 설치가 진행됩니다.  

Windows의 경우, 설치 프로그램을 실행하면 Windows Defender SmartScreen 경고 메시지가 발생할 수 있습니다.  
게시자에 **AFI, Inc**가 표시되는 경우 위변조되지 않고 인증된 정상적인 파일이니 안심하셔도 됩니다.  

> 원하는 경우 설치 파일 우클릭 > 속성 > 디지털 서명에서 인증 내역을 확인할 수 있습니다.  

![image](https://developer.thebackend.io/static/img/bfunc/install/0.png)

**설치 완료 후 반드시 컴퓨터 재부팅을 진행**해야 설치가 정상적으로 완료됩니다.  

![image](/img/docs/guide/function/intro/new-install-result.png)

### 뒤끝펑션 프로젝트 템플릿 설치하기

실행 중인 모든 비주얼 스튜디오 인스턴트를 종료하신 후 BackendFunction.vsix 파일을 실행하면 설치가 진행됩니다.  
현재 개발 환경에 설치되어 있는 Visual Studio 버전이 표시됩니다.  

### VSCode 용 뒤끝펑션 확장 설치하기

Visual Studio Code 내 확장에서 검색을 이용해 BackendFunction을 검색 후, BackendFunction을 찾아 설치합니다.  
또는 [웹페이지 Vscode 내 MarketPlace](https://marketplace.visualstudio.com/items?itemName=AFI.backendfunction)에서 Install 버튼을 눌러 설치합니다.  

![image](/img/docs/guide/function/intro/install-extension.png)
