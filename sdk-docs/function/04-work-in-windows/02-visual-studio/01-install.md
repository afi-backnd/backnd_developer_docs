---
sidebar_label: Visual Studio 준비
description: "Visual Studio 준비"
---

# Visual Studio 준비하기

Visual Studio에서 뒤끝펑션 프로젝트를 열고 디버깅하기 위해 필요한 구성 요소입니다.  
뒤끝펑션 전용 프로젝트 템플릿(VSIX)은 더 이상 필요하지 않습니다. 프로젝트는 `backend new` 명령으로 만듭니다.  

## 필요한 구성 요소

**Visual Studio 2022 17.8 이상**에서 **Visual Studio Installer**를 실행해 아래 항목을 설치합니다.  

### 워크로드

- .NET 데스크톱 개발

### 개별 구성 요소

- .NET 8.0 런타임
  > .NET SDK로 표시될 수도 있습니다.  

## 설치 확인

[backend를 설치](/sdk-docs/function/work-in-windows/install)한 뒤 PowerShell에서 아래 명령을 실행합니다.  

```powershell
dotnet --list-sdks
dotnet --list-runtimes
```

`dotnet --list-sdks` 목록에 `8.0` 이상 버전의 SDK가 있고, `dotnet --list-runtimes` 목록에 `Microsoft.NETCore.App 8.`로 시작하는 항목이 있으면 준비가 완료된 것입니다.  
Visual Studio에서 프로젝트를 빌드하고 디버깅하려면 PC에 .NET 8 이상 SDK와 .NET 8 런타임이 설치되어 있어야 합니다. `backend`가 내려받는 전용 SDK는 Visual Studio의 빌드에는 사용되지 않습니다.  
둘 중 하나가 없으면 [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8.0)를 설치합니다.  

:::info 이전에 설치한 뒤끝펑션 템플릿
1.0.2 이하 버전에서 설치한 BackendFunction 프로젝트 템플릿은 1.1.0부터 사용하지 않습니다.  
**확장 > 확장 관리**에서 제거해도 됩니다.  
:::
