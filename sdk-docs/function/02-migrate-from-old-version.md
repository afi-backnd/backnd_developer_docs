---
sidebar_label: 뒤끝펑션 버전 마이그레이션  
---

# 뒤끝펑션 1.0.2 으로 업데이트
기존에 사용하시던 뒤끝 펑션 프로젝트를 최신 버전으로 마이그레이션하는 가이드 입니다.
:::caution .NET 6.0 사용 중단 알림

뒤끝펑션에 사용되는 .NET 6.0의 AWS 서비스 지원이 단계적으로 중단됩니다.  
2025년 2월 28일부터는 .NET 6.0을 사용하여 새로운 함수를 생성할 수 없습니다.  
2025년 3월 31일부터는 기존에 업로드 되어 있던 함수를 .NET 6.0을 사용해서 업데이트 할 수 없습니다.  
지원 중단 이 후에도 기존에 있던 함수는 계속 실행 가능하지만 0.4.1 이하 버전으로는 업데이트가 제한되며  
0.5.0 이상 버전을 사용해야만 업데이트가 가능합니다.  

기존 뒤끝펑션 프로젝트를 정상적으로 업로드하기 위해서는 아래의 작업을 완료해야 합니다.

* .NET 6.0 > .NET 8.0으로 업데이트  
* 뒤끝펑션 0.5.0 이상 버전으로 재설치  
* Backend.dll파일 최신버전으로 교체  
:::

## 1. .NET 8.0으로 변경
[.NET 8.0](https://dotnet.microsoft.com/download/dotnet/8.0)으로 업데이트 후, 프로젝트의 TargetFramework를 .NET 8.0으로 변경해야 합니다.  

### Visual Studio의 경우
1. BackendFunction.csproj 우클릭, 속성을 클릭합니다.  
![image](https://developer.thebackend.io/static/img/bfunc/install/bf3.1-studio1.PNG)


2. 애플리케이션 > 대상 프레임워크를 .NET 8.0로 변경합니다.  
![image](/img/docs/guide/function/migrate/vs-dotnet.png)

만약 해당 버전의 선택이 불가능할 경우 [.NET 8.0](https://dotnet.microsoft.com/download/dotnet/8.0)을 다운 받고 컴퓨터를 재시작해주시기 바랍니다.  

### VSCode의 경우
1. BackendFunction.csproj에서 TargetFramework를 **net8.0**로 변경합니다.  
   > ![image](/img/docs/guide/function/migrate/vsc-dotnet.png)

## 2. 뒤끝펑션 1.0.2 버전 설치
뒤끝펑션 개발 툴 1.0.2 버전을 다운로드 하고 실행합니다.  
* [뒤끝펑션 개발 툴 for Windows](https://developer.thebackend.io/sdk/BackendFunction/1.0.2/Setup.exe) \[2025-08-08]
* [뒤끝펑션 개발 툴 for Mac-arm64](https://developer.thebackend.io/sdk/BackendFunction/1.0.2/SetupForMac-arm64.pkg) \[2025-08-08]
* [뒤끝펑션 개발 툴 for Mac-x64](https://developer.thebackend.io/sdk/BackendFunction/1.0.2/SetupForMac-x64.pkg) \[2025-08-08]

![image](/img/docs/guide/function/migrate/new-install-finish.png)  

설치가 완료되면 컴퓨터를 재시작합니다.  

## 3. Backend.dll 파일 교체
0.5.0이하 버전에서 사용 중이던 BackendAPI.dll파일을 Backend.dll파일로 교체하여 프로젝트에 적용해야 합니다.  
[펑션 SDK 업그레이드](/sdk-docs/function/upgrade)를 참고하여 DLL 파일 교체를 진행합니다.  
  

## 4. 펑션 템플릿 Function함수 동기/비동기 선택 가능
펑션 디버거가 동기/비동기 테스트를 지원하도록 변경 되었습니다.  
[펑션 SDK 업그레이드](/sdk-docs/function/upgrade#7)의 7번항목을 참고하여 개발 상황에 따라 변경해 주세요.  


### 정상적으로 동작하지 않을 경우

만약 업그레이드 후 정상적으로 동작하지 않을 경우 기존에 깔려있던 뒤끝펑션을 삭제하고 다시 설치해주세요.  
뒤끝펑션을 제거하는 방법은 Program Files > AFI > uninst.exe

![image](/img/docs/guide/function/migrate/delete-confirm.png)
