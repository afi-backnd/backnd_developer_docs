---
sidebar_label: 설치하기
description: "설치하기"
---

# Windows에 설치하기

뒤끝펑션 개발 도구 `backend`를 Windows에 설치하는 방법입니다.  
관리자 권한 없이 현재 사용자 계정에만 설치됩니다.  

## 준비 사항

- Windows 10 이상(64비트), Windows PowerShell 5.1 이상
- [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8.0) (권장)
  - 설치되어 있지 않으면 `backend`가 사용자 폴더에 전용 SDK를 내려받습니다. 편집기의 코드 자동 완성과 디버깅을 사용하려면 직접 설치해 주세요.

:::danger 1.0.2 이하 버전을 먼저 제거해 주세요
1.0.2 이하 버전의 개발 도구(Setup.exe로 설치)가 남아 있으면 `backend`를 입력했을 때 새 버전 대신 기존 버전이 실행됩니다.  
기존 버전이 새 `backend`보다 먼저 실행되는 환경이면 설치 명령은 **아무것도 설치하지 않고 멈춥니다.** 이때 나오는 안내는 아래 [기존 버전 때문에 설치가 멈춘 경우](#기존-버전-때문에-설치가-멈춘-경우)를 참고해 주세요.  
**설정 > 앱 > 설치된 앱**에서 BackendFunction을 제거하거나 `C:\Program Files\AFI\BackendFunction\uninst.exe`를 실행해 제거한 뒤 설치해 주세요.  
기존 프로젝트를 옮기는 방법은 [기존 버전에서 옮기기](/sdk-docs/function/migrate-from-old-version) 문서를 참고해 주세요.  
:::

## 1. 설치하기

시작 메뉴에서 **PowerShell**을 열고 아래 명령을 실행합니다.  

```powershell
irm https://developer.thebackend.io/sdk/BackendFunction/live/install.ps1 | iex
```

설치 스크립트는 아래 순서로 진행합니다.  

1. 기존 버전이 새 `backend`보다 먼저 실행되는지 확인합니다. 그렇다면 여기서 멈춥니다.  
2. 최신 버전을 확인하고 설치 파일을 내려받아, **AFI, INC.의 코드 서명**을 확인한 뒤에만 실행합니다. 자세한 내용은 [설치 파일의 코드 서명](#설치-파일의-코드-서명)을 참고해 주세요.  
3. 실행된 설치 파일이 `backend`를 설치합니다. `backend`는 릴리스의 서명을 한 번 더 확인합니다.  
4. `%LOCALAPPDATA%\AFI\Function\bin`을 사용자 PATH 앞에 추가합니다.  

설치가 끝나면 아래와 같은 메시지가 표시됩니다. 버전과 사용자 폴더 경로는 환경에 따라 다릅니다.  

```text
Latest release in this channel: 1.1.1
BACKND Function CLI 1.1.1 was installed in C:\Users\<사용자 이름>\AppData\Local\AFI\Function.
Added C:\Users\<사용자 이름>\AppData\Local\AFI\Function\bin to the user PATH.
Open a new terminal, then run: backend --version
```

이미 설치되어 있다면 `BACKND Function CLI 1.1.1 is already installed in ...`이 표시됩니다. 같은 명령을 다시 실행해도 안전하며, 사용자 PATH에서 설치 폴더가 빠져 있었다면 다시 추가합니다.  

## 2. 설치 확인하기

**새 PowerShell 창**을 열고 아래 명령을 실행합니다.  
설치 명령을 실행한 창에서도 바로 사용할 수 있지만, 앞으로 사용할 환경과 같은 새 창에서 확인합니다.  

```powershell
backend --version
```

아래와 같이 버전이 표시되면 설치가 완료된 것입니다.  

```text
backend 1.1.1
```

방금 설치한 `backend`가 실행되는지도 확인합니다. 아래 명령의 결과가 `%LOCALAPPDATA%\AFI\Function\bin\backend.exe` 경로여야 합니다.  

```powershell
(Get-Command backend).Source
```

`C:\Program Files\AFI\BackendFunction\CLI\Backend.exe`가 표시되면 기존 버전이 실행되고 있는 것입니다. 기존 버전을 제거한 뒤 창을 다시 엽니다.  

개발 환경 전체를 점검하려면 아래 명령을 실행합니다.  
결과의 `"ok": true`와 `"dotnet"` 항목의 `"ready": true`를 확인합니다.  

```powershell
backend doctor
```

:::caution 이미 열려 있던 프로그램에서는 backend를 찾지 못합니다
PATH는 새로 여는 프로그램부터 적용됩니다.  
설치 전에 열어 둔 PowerShell, 명령 프롬프트, Visual Studio, Visual Studio Code에서 `backend`를 찾지 못하면 프로그램을 모두 닫고 다시 열어 주세요.  
:::

## 3. 인증 정보 등록하기

설치를 확인했다면 `backend config`로 뒤끝펑션 인증키를 등록합니다. 처음 한 번만 하면 되고, 이후에 만드는 모든 프로젝트에서 사용합니다.  

```powershell
backend config
```

`Function auth key` 질문에 뒤끝 콘솔에서 복사한 인증키를 붙여 넣고(화면에는 표시되지 않습니다), 나머지 질문은 Enter를 누릅니다.  
함수에서 뒤끝 기능을 호출하고 이를 내 PC에서 테스트 계정으로 실행하려면 클라이언트 앱 ID, 시그니처 키, 패키지 이름도 입력하고 테스트 계정으로 로그인합니다. 자세한 내용은 [인증 정보 등록](/sdk-docs/function/cli/setting) 문서를 참고해 주세요.  

등록을 마쳤다면 [Visual Studio Code](/sdk-docs/function/work-in-windows/visual-studio-code/create) 또는 [Visual Studio](/sdk-docs/function/work-in-windows/visual-studio/create)에서 프로젝트를 만듭니다.  

## 기존 버전 때문에 설치가 멈춘 경우

기존 버전(`C:\Program Files\AFI\BackendFunction\CLI`)이 시스템 PATH에 남아 있으면 새 터미널에서 `backend`를 입력했을 때 기존 버전이 실행됩니다. 설치 명령은 이 경우를 미리 확인하고 아래 메시지를 표시한 뒤 멈춥니다. 파일은 하나도 만들지 않고 인터넷에서 아무것도 내려받지 않습니다.  

```text
BACKEND_INSTALL_LEGACY_SHADOW: 'backend' in a new terminal would run
  C:\Program Files\AFI\BackendFunction\CLI\Backend.exe
instead of the new BACKND Function CLI. Nothing was installed.
Remove the old BACKND Function tools first:
  1. Settings > Apps > Installed apps > uninstall the old BACKND Function CLI, or
  2. (administrator) remove the folder of the old CLI from the system PATH.
Then run the installer again. To install anyway and call the new CLI by its full path,
set BACKEND_INSTALL_ALLOW_SHADOW=1 (or pass -AllowShadowed) and run the installer again.
```

PowerShell이 이 메시지 아래에 `+ CategoryInfo ...` 같은 오류 정보를 함께 표시할 수 있습니다. 확인할 내용은 위 메시지입니다.  

기존 버전이 아닌 다른 프로그램의 `backend` 파일이 새 `backend`보다 먼저 실행되는 경우에도 같은 메시지로 멈춥니다. 메시지 둘째 줄에 먼저 실행되는 파일의 경로가 표시됩니다.  

**해결 방법:** 메시지의 1번대로 **설정 > 앱 > 설치된 앱**에서 기존 BackendFunction을 제거합니다. 그 뒤 새 PowerShell 창에서 설치 명령을 다시 실행합니다. 시스템 PATH를 직접 고치는 2번은 관리자 권한이 필요하므로 제거가 가능하다면 1번을 권장합니다.  

기존 버전을 제거하지 않고 새 버전을 설치하려면 `BACKEND_INSTALL_ALLOW_SHADOW`를 `1`로 설정하고 설치 명령을 다시 실행합니다.  

```powershell
$env:BACKEND_INSTALL_ALLOW_SHADOW = '1'
irm https://developer.thebackend.io/sdk/BackendFunction/live/install.ps1 | iex
```

:::caution 재정의하면 기존 버전이 계속 먼저 실행됩니다
재정의하면 설치는 끝나지만 새로 연 터미널과 편집기에서는 **기존 버전이 계속 실행됩니다.** 설치하는 동안 아래 경고가 두 번 표시됩니다. 한국어 Windows에서는 `WARNING:` 대신 `경고:`로 표시됩니다.  

```text
WARNING: C:\Program Files\AFI\BackendFunction\CLI\Backend.exe comes before the new CLI on the PATH; continuing because the shadow check was overridden.
Latest release in this channel: 1.1.1
BACKND Function CLI 1.1.1 was installed in C:\Users\<사용자 이름>\AppData\Local\AFI\Function.
Added C:\Users\<사용자 이름>\AppData\Local\AFI\Function\bin to the user PATH.
WARNING: New terminals still start C:\Program Files\AFI\BackendFunction\CLI\Backend.exe; run C:\Users\<사용자 이름>\AppData\Local\AFI\Function\bin\backend.exe directly.
Open a new terminal, then run: backend --version
```

기존 버전은 동작과 인증 정보 저장 방식이 새 버전과 달라 `backend deploy`가 다르게 동작할 수 있습니다.  
새 버전을 사용하려면 `backend` 대신 `%LOCALAPPDATA%\AFI\Function\bin\backend.exe`를 전체 경로로 실행해야 합니다. Visual Studio Code의 F5 디버깅은 `backend`를 이름으로 실행하므로 정상 동작하지 않을 수 있습니다. 가능하면 기존 버전을 제거해 주세요.  
:::

기존 버전이 있지만 새 `backend`보다 먼저 실행되지 않는다면 멈추지 않고 `Note: an older BACKND Function CLI exists at ... but does not come first on the PATH.`만 표시합니다.  

## 설치 옵션

대부분은 옵션 없이 설치하면 됩니다. 필요한 경우에만 사용해 주세요.  

| 목적 | 환경 변수 (`irm ... \| iex`에서 사용) | 옵션 (스크립트 블록 형태에서 사용) |
|---|---|---|
| 사용자 PATH를 바꾸지 않습니다. 설치 뒤 `backend.exe`를 전체 경로로 실행합니다. | `BACKEND_INSTALL_NO_PATH=1` | `-NoPath` |
| 설치 폴더를 바꿉니다. | `BACKEND_HOME=<절대 경로>` | `-InstallRoot <절대 경로>` |
| 기존 버전이 먼저 실행되어도 설치합니다. | `BACKEND_INSTALL_ALLOW_SHADOW=1` | `-AllowShadowed` |

- `irm ... | iex`는 옵션을 전달할 수 없으므로 환경 변수를 사용합니다. 환경 변수는 해당 PowerShell 창에서만 적용됩니다.  
- 옵션은 아래처럼 스크립트 블록 형태로 실행할 때 사용합니다.  

  ```powershell
  & ([scriptblock]::Create((irm https://developer.thebackend.io/sdk/BackendFunction/live/install.ps1))) -NoPath
  ```

- 설치 폴더는 `;`, `%`, `"`가 없는 절대 경로여야 하고, 현재 사용자·SYSTEM·Administrators만 쓸 수 있는 위치여야 합니다. 다른 사용자도 파일을 바꿀 수 있는 폴더에는 설치할 수 없습니다.  
- `-NoPath`로 설치하면 기존 버전 확인도 하지 않습니다.  

## 설치 파일의 코드 서명

설치 명령이 내려받는 설치 파일과 `backend.exe`에는 뒤끝의 운영사 **AFI, INC.**의 코드 서명이 있습니다. 서명 인증서는 공인 인증 기관 GlobalSign이 발급했습니다.  
설치 명령은 내려받은 설치 파일의 서명을 확인한 뒤에만 실행하고, `backend update`도 새 버전의 서명을 확인한 뒤에만 설치합니다. 서명을 확인하지 못하면 아무것도 설치하지 않고 멈춥니다.  

설치한 `backend.exe`의 서명은 아래 명령으로 확인할 수 있습니다. `Status`가 `Valid`이고 `Subject`에 `AFI, INC.`가 표시되면 정상입니다.  

```powershell
Get-AuthenticodeSignature "$env:LOCALAPPDATA\AFI\Function\bin\backend.exe" | Format-List Status, SignerCertificate
```

파일 탐색기에서 `backend.exe`의 **속성 > 디지털 서명** 탭으로도 확인할 수 있습니다.  

## 스크립트를 확인한 뒤 설치하기

회사 보안 정책 등으로 실행하기 전에 설치 스크립트를 검토해야 한다면, 파일로 내려받아 내용을 확인한 뒤 실행합니다.  

```powershell
irm https://developer.thebackend.io/sdk/BackendFunction/live/install.ps1 -OutFile "$env:TEMP\backend-install.ps1"
notepad "$env:TEMP\backend-install.ps1"
& ([scriptblock]::Create((Get-Content -Raw "$env:TEMP\backend-install.ps1")))
```

## 회사 네트워크에서 설치가 실패하는 경우

회사 보안 장비가 HTTPS 통신을 중간에서 검사하는 환경에서는 설치가 `NETWORK_ERROR`로 끝날 수 있습니다.  
`backend`는 Windows 인증서 저장소가 아니라 `backend`에 포함된 인증서 목록으로 HTTPS 연결을 확인합니다.  
네트워크 담당자에게 `developer.thebackend.io`를 HTTPS 검사 예외로 등록해 달라고 요청해 주세요.  

## 설치 위치

| 항목 | 위치 |
|---|---|
| 설치 폴더 | `%LOCALAPPDATA%\AFI\Function` |
| 실행 파일 | `%LOCALAPPDATA%\AFI\Function\bin\backend.exe` |
| 설치된 버전 | `%LOCALAPPDATA%\AFI\Function\versions` |
| 내려받은 .NET SDK와 디버거 | `%LOCALAPPDATA%\AFI\Function\tools` |
| 등록한 인증 정보와 테스트 계정 로그인 정보 | `%LOCALAPPDATA%\AFI\Function\profiles` (현재 Windows 사용자만 읽을 수 있도록 암호화) |

## 업데이트하기

새 버전이 배포되면 아래 명령으로 업데이트합니다.  
새 버전을 확인한 뒤 확인 질문이 나오면 `y`를 입력합니다.  

```powershell
backend update
```

업데이트는 새 버전을 별도 폴더에 설치한 뒤 전환하므로 등록한 인증 정보와 프로젝트는 그대로 유지됩니다.  
프로젝트에 포함된 SDK DLL은 자동으로 바뀌지 않습니다. 화면 예시와 실패했을 때의 해결 방법은 [업데이트](/sdk-docs/function/upgrade) 문서를 참고해 주세요.  

## 제거하기

PowerShell에서 아래 명령을 실행하면 사용자 PATH에서 설치 폴더를 빼고 설치 폴더를 삭제합니다. PATH의 다른 항목(`%USERPROFILE%` 같은 변수 포함)은 그대로 둡니다.  
**등록한 인증 정보와 테스트 계정 로그인 정보도 함께 삭제됩니다.** 둘 다 설치 폴더의 `profiles` 폴더에 저장되어 있기 때문입니다. 다시 설치하면 `backend config`로 다시 등록해야 합니다.  

```powershell
$root = Join-Path $env:LOCALAPPDATA 'AFI\Function'
$bin = Join-Path $root 'bin'
$key = [Microsoft.Win32.Registry]::CurrentUser.OpenSubKey('Environment', $true)
$raw = $key.GetValue('Path', '', 'DoNotExpandEnvironmentNames')
$kind = $key.GetValueKind('Path')
$entries = $raw -split ';' | Where-Object { $_ -and ([Environment]::ExpandEnvironmentVariables($_).TrimEnd('\') -ne $bin) }
$key.SetValue('Path', ($entries -join ';'), $kind)
$key.Close()
Remove-Item -Recurse -Force $root
```

- 설치 폴더를 바꿔 설치했다면 위 명령의 `$root`를 그 폴더로 바꿔 실행해 주세요.  
- 삭제되는 것은 이 PC에 저장된 값뿐입니다. `backend`는 제거할 때 서버에 로그아웃을 요청하지 않습니다.  
- 함수 프로젝트 폴더는 삭제되지 않습니다. 프로젝트 안의 `.backend` 폴더(빌드·디버깅 파일)는 필요하면 직접 삭제해 주세요. 다시 실행하면 자동으로 만들어집니다.  
- `Remove-Item`이 파일을 사용 중이라는 오류를 내면 `backend`를 실행 중인 PowerShell과 편집기를 모두 닫고 다시 실행합니다.  
