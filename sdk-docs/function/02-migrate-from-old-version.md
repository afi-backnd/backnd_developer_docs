---
sidebar_label: 기존 버전에서 옮기기
description: "기존 버전에서 옮기기"
---

# 기존 버전에서 옮기기

1.0.2 이하 버전에서 만든 뒤끝펑션 프로젝트를 1.1.0 이상 버전에서 사용하는 방법입니다.

함수 코드(`Function.cs`)는 그대로 사용할 수 있고, 프로젝트 설정만 1.1.0 형식으로 옮기면 됩니다.  

## 바뀐 점

| 항목 | 1.0.2 이하 | 1.1.0 이상 |
|---|---|---|
| 설치 | Setup.exe(Windows), pkg(Mac) | 설치 명령 한 줄 ([Windows](/sdk-docs/function/work-in-windows/install) / [Mac](/sdk-docs/function/work-in-ios/install)) |
| 프로젝트 생성 | Visual Studio 템플릿(VSIX), Visual Studio Code BackendFunction 확장 | `backend new` |
| 디버깅 | BF Debug Tool | 편집기의 F5 디버깅 또는 `backend debug` |
| 디버깅 입력 값 | `debugConfig.json`의 `content` | `requests/default.json` (Git에 올라가는 파일) |
| 디버깅 실행 방식 | `debugConfig.json`의 `mode`(생략하면 테스트 계정으로 로그인) | 기본은 인증 정보 없이 실행(오프라인). 테스트 계정은 `backend debug --user` |
| 프로젝트 정보 | PC 하나에 config.json 하나 (`backend config`로 편집) | 프로젝트 폴더마다 `backend-function.json` |
| 인증 정보 | config.json과 `debugConfig.json`에 그대로 저장 | `backend config`로 사용자 계정에 암호화해 저장 |
| SDK 업데이트 | 설치 폴더의 DLL을 직접 복사 | `backend update --project` |

## 1. 기존 함수 이름 확인하기

1.1.0부터는 프로젝트 폴더 이름이 기본 함수 이름이 됩니다.  
기존에 배포한 함수를 같은 이름으로 다시 배포하려면 그 이름을 먼저 확인해 둡니다.  

- 뒤끝 콘솔의 뒤끝펑션 메뉴에서 배포된 함수 이름을 확인합니다.  
- 또는 기존 설정 파일의 `functionName`을 확인합니다. Windows는 `%APPDATA%\AFI\BFunc\config.json`, Mac은 `/Library/Application Support/AFI/BFunc/config.json`입니다.  

Windows에서는 기존 개발 도구를 제거하면 config.json도 함께 삭제되므로, 제거하기 전에 확인해 주세요.  

## 2. 기존 개발 도구 제거하기

**Windows (필수)**  
기존 개발 도구가 남아 있으면 `backend`를 입력했을 때 새 버전 대신 기존 버전이 실행됩니다. 새 버전의 설치 명령도 이 경우 설치를 멈추고 제거를 안내합니다.  
**설정 > 앱 > 설치된 앱**에서 BackendFunction을 제거하거나 `C:\Program Files\AFI\BackendFunction\uninst.exe`를 실행합니다.  

**Mac (권장)**  
기존 개발 도구가 `~/.zshrc`에 남긴 PATH 줄(`AFI/BFunc`) 때문에 새 터미널에서 기존 `backend`가 먼저 실행될 수 있습니다. 이 경우 새 버전의 설치 명령은 PATH 확인에서 실패하고 안내를 표시합니다.  
지우려면 응용 프로그램의 `AFI` 폴더를 휴지통으로 옮긴 뒤 [뒤끝펑션 환경 변수 제거 프로그램](https://developer.thebackend.io/sdk/BackendFunction/0.3.0/uninstallForMac)을 실행합니다.  

**편집기 확장 (선택)**  
Visual Studio의 BackendFunction 프로젝트 템플릿과 Visual Studio Code의 BackendFunction 확장은 1.1.0부터 사용하지 않으므로 제거해도 됩니다.  

## 3. 새 버전 설치하기

[Windows](/sdk-docs/function/work-in-windows/install) / [Mac](/sdk-docs/function/work-in-ios/install) 설치 문서를 참고해 설치합니다.  

## 4. .NET 8 프로젝트인지 확인하기

`BackendFunction.csproj`를 열어 `TargetFramework`가 `net8.0`인지 확인합니다.  
`net6.0` 등 다른 값이면 `net8.0`으로 바꾸고 저장합니다.  

```xml
<TargetFramework>net8.0</TargetFramework>
```

## 5. 프로젝트 옮기기

PowerShell 또는 터미널에서 기존 프로젝트 폴더(`BackendFunction.csproj`가 있는 폴더)로 이동합니다.  
1번에서 확인한 함수 이름을 `--name`으로 지정해 아래 명령을 실행합니다.  

```powershell
cd C:\work\MyOldFunction
backend migrate --name myFunction
```

`--yes` 없이 실행하면 프로젝트를 바꾸지 않고 바뀔 파일만 보여 줍니다. 아래는 기본 템플릿으로 만든 프로젝트의 결과입니다. 프로젝트마다 `changes` 목록은 다를 수 있습니다.  

```json
{
  "command": "migrate",
  "data": {
    "applied": false,
    "approvalRequired": true,
    "changes": [
      {
        "action": "create",
        "path": "backend-sdk.json",
        "reason": "Record actual existing DLL hashes without replacing any SDK files"
      },
      {
        "action": "replaceWithBackup",
        "path": "BackendFunction.csproj",
        "reason": "Remove recognized debugger/ZIP shell targets and prepare project-local Debug output while preserving package references"
      },
      {
        "action": "create",
        "path": "backend-function.json",
        "reason": "Create project-local manifest without importing credentials or claiming a bundled SDK"
      },
      {
        "action": "replaceWithBackup",
        "path": ".vscode/launch.json",
        "reason": "Replace recognized legacy debugger entries, preserve custom IDE entries and drop generated machine paths"
      },
      {
        "action": "replaceWithBackup",
        "path": ".vscode/tasks.json",
        "reason": "Replace recognized legacy debugger entries, preserve custom IDE entries and drop generated machine paths"
      },
      {
        "action": "replaceWithBackup",
        "path": "Properties/launchSettings.json",
        "reason": "Replace recognized old debugger profiles, preserve custom profiles and drop generated machine paths"
      },
      {
        "action": "create",
        "path": "requests/default.json",
        "reason": "Copy the legacy debugConfig.json content, byte for byte, into the default request"
      },
      {
        "action": "create",
        "path": ".gitignore",
        "reason": "Protect project-local build state and preserved debug credentials"
      }
    ],
    "kept": [],
    "note": "Migration never imports legacy credentials. Pass --yes to apply the previewed changes.",
    "notes": [
      "backend debug now runs offline by default; use --user to run as the test user. A legacy file runs only with --config.",
      "requests/default.json will be tracked by Git (it is not in .gitignore). Review it before committing; the legacy debugConfig.json was ignored."
    ],
    "preserved": [
      "Function.cs",
      "dll/",
      "debugConfig.json"
    ],
    "project": "C:\\work\\MyOldFunction"
  },
  "error": null,
  "ok": true,
  "schemaVersion": 1
}
```

| 항목 | 설명 |
|---|---|
| `changes` | 바뀔 파일입니다. `create`는 새로 만들고, `replaceWithBackup`은 백업한 뒤 바꿉니다. |
| `kept` | 직접 수정한 내용이 있어 바꾸지 않고 그대로 둔 IDE 설정입니다. |
| `notes` | 옮긴 뒤 알아야 할 내용입니다. 꼭 읽어 주세요. |
| `preserved` | 바꾸지 않는 파일입니다. 함수 코드, SDK DLL, `debugConfig.json`은 그대로 유지됩니다. |

내용을 확인했다면 `--yes`를 추가해 적용합니다.  

```powershell
backend migrate --name myFunction --yes
```

결과에 `"applied": true`가 표시되면 완료된 것입니다.  
바꾸기 전 파일은 `.backend/backups` 폴더에 백업됩니다.  

- 기존 빌드 설정 중 이전 디버거(BF Debug Tool)와 ZIP 생성 설정을 제거합니다.  
- Visual Studio와 Visual Studio Code의 디버깅 설정을 1.1.0 방식으로 바꿉니다. **오프라인**과 **테스트 계정** 두 가지 실행 구성이 만들어지며, 직접 추가한 다른 설정은 유지합니다.  
- `debugConfig.json`의 `content`를 `requests/default.json`으로 복사합니다. `requests/default.json`이 이미 있으면 덮어쓰지 않습니다.  
- `.gitignore`에 `.backend/`와 `debugConfig.json`을 추가합니다. 파일이 없으면 새로 만듭니다.  
- **인증 정보는 옮기지 않습니다.** `debugConfig.json`은 지우거나 바꾸지 않고 그대로 둡니다. 인증 정보는 7번에서 등록합니다.  

## 6. 요청 파일 만들기

1.1.0부터 `backend debug`는 `debugConfig.json`을 읽지 않고 `requests/default.json`을 읽습니다.  
5번에서 `content`가 자동으로 복사되었으므로 파일을 열어 함수에 전달할 값이 맞는지 확인합니다.  

```json
{
  "value": "HelloWorld!"
}
```

- 요청 파일은 `.gitignore`에 포함되지 않으므로 **Git에 올라갑니다.** 인증 정보가 들어 있지 않은지 확인한 뒤 커밋해 주세요.  
- `content`가 JSON 객체가 아니거나 `debugConfig.json`에 적힌 인증 정보와 같은 값이 들어 있으면 복사하지 않고 `notes`에 안내합니다. 이 경우 `requests/default.json`을 직접 만들어 주세요.  
- 기본 실행은 인증 정보 없이 실행하는 **오프라인** 방식입니다. 예전에는 `mode`를 적지 않으면 테스트 계정으로 로그인해 실행했지만 이제는 `backend debug --user`로 실행합니다.  
- 제한 시간은 `timeoutEnable`을 사용하지 않고 기본 10초이며, `backend debug --timeout`으로 바꿉니다. [타임아웃](/sdk-docs/function/basic/timeout) 문서를 참고해 주세요.  
- 입력 값을 여러 개 두려면 `requests` 폴더에 파일을 더 만들고 `backend debug --data 파일이름`으로 실행합니다. 자세한 내용은 [로컬 실행](/sdk-docs/function/cli/debug#요청-파일) 문서를 참고해 주세요.  

옮긴 뒤 `debugConfig.json`은 `--config`를 직접 지정할 때만 사용됩니다.  

```powershell
backend debug --config debugConfig.json
```

이 방식은 예전 규칙을 그대로 따르므로 아래 내용에 주의해 주세요. 새로 만든 요청 파일로 실행하는 것을 권장합니다.  

- `mode`가 없으면 테스트 계정으로 로그인해 실제 서버에서 실행하므로 실제 데이터가 바뀔 수 있습니다. 로그인 없이 실행하려면 `"mode": "offline"`을 추가합니다.  
- `timeoutEnable`은 `true` 또는 `false`여야 합니다. 빈 값(`""`)이면 `CONFIG_INVALID` 오류가 나므로 값을 넣거나 항목을 지웁니다. 기본 템플릿에 들어 있던 빈 값이 그 예입니다.  
- 이전 문서의 예시처럼 `//` 주석이 들어 있으면 `CONFIG_INVALID` 오류가 납니다. 주석을 모두 지워 주세요.  

각 항목의 설명은 [로컬 실행](/sdk-docs/function/cli/debug#예전-debugconfigjson으로-실행하기) 문서를 참고해 주세요.  

## 7. 인증 정보 등록하기

`backend migrate`는 인증 정보를 옮기지 않습니다. 프로젝트 폴더에서 `backend config`를 실행해 등록합니다.  

```powershell
backend config
```

`debugConfig.json`에 인증 정보가 적혀 있다면 파일을 발견했다는 질문이 나옵니다.  
`y`를 입력하면 값이 비어 있는 항목의 기본값으로 파일의 값을 가져와, Enter만 눌러도 저장됩니다.  

```text
Found credentials in C:\work\MyOldFunction\debugConfig.json. Offer them as defaults for empty fields? [y/N]:
```

- 뒤끝펑션 인증키가 `debugConfig.json`에 없다면 뒤끝 콘솔에서 복사해 입력합니다.  
- 파일에 테스트 계정 ID와 비밀번호가 있으면 프로필을 새로 만들 때(`--reset`으로 처음부터 다시 입력할 때 포함) 로그인 질문의 기본값으로 사용합니다. 이미 저장된 프로필에 `backend config`를 다시 실행할 때는 사용하지 않으므로 직접 입력합니다. 비밀번호는 저장하지 않고 로그인 정보(토큰)만 저장합니다.  
- 가져온 뒤에는 `debugConfig.json`에서 인증 정보를 지워 주세요. 인증 정보가 남아 있는 동안은 `backend config`를 실행할 때마다 같은 질문이 나옵니다.  

질문과 입력 방법은 [인증 정보 등록](/sdk-docs/function/cli/setting) 문서를 참고해 주세요.  

## 8. SDK DLL 업데이트하기

옮긴 프로젝트는 기존 SDK DLL을 그대로 사용합니다.  
설치한 `backend`에 포함된 SDK로 바꾸려면 프로젝트 폴더에서 아래 명령을 실행합니다.  
`--yes` 없이 먼저 실행해 바뀔 DLL을 확인할 수 있습니다.  

```powershell
backend update --project . --yes
```

자세한 내용은 [업데이트](/sdk-docs/function/upgrade#프로젝트-sdk-업데이트하기) 문서를 참고해 주세요.  

:::caution 기존 함수 코드의 using 문
코드에 `using BackendAPI;` 또는 `using BackendAPI.Value;`가 있으면 빌드 오류가 납니다.  
`using BackEnd;`로 바꿔 주세요. 자세한 내용은 [뒤끝펑션 템플릿](/sdk-docs/function/template) 문서를 참고해 주세요.  
:::

## 9. 실행하고 배포하기

프로젝트 폴더에서 내 PC 실행과 배포를 확인합니다.  

```powershell
backend debug
backend deploy
```

`backend debug` 결과의 `execution.ok`가 `true`인지 확인합니다.  
`backend deploy`의 확인 질문에 표시되는 함수 이름이 기존에 배포한 함수 이름과 같은지 확인한 뒤 `y`를 입력합니다.  

## 옮기기에 실패한 경우

| 오류 코드 | 원인과 해결 방법 |
|---|---|
| `AMBIGUOUS_PROJECT` | 폴더에 `.csproj` 파일이 여러 개 있습니다. `--project`로 옮길 `.csproj` 파일을 지정합니다. |
| `MIGRATION_UNSAFE` | 자동으로 판단할 수 없는 빌드 설정이 `.csproj`에 남아 있습니다. 직접 추가한 빌드 후 실행 명령 등을 확인해 정리한 뒤 다시 실행합니다. |
| `SDK_CHANGED` | `dll` 폴더의 DLL이 기록과 다릅니다. 직접 바꾼 DLL이 있는지 확인합니다. |
| `PROJECT_CHANGED` | 옮기는 중에 파일이 바뀌었습니다. 편집기에서 저장 중인 파일이 없는지 확인한 뒤 다시 실행합니다. |
| `MIGRATION_REQUIRED` | `BackendFunction.csproj`에 이전 디버거·ZIP 생성 설정이 남아 있어 빌드할 수 없습니다. `backend migrate`로 확인한 뒤 `--yes`로 적용합니다. |
| `REQUEST_MISSING` | `backend debug`가 `requests/default.json`을 찾지 못했습니다. `backend migrate`를 실행하지 않았거나 파일을 옮기지 못한 경우입니다. 6번을 참고해 파일을 만듭니다. |
