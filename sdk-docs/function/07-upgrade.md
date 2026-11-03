---
sidebar_label: 업데이트
description: "업데이트"
---

# 업데이트

뒤끝펑션 업데이트는 두 가지입니다.  

| 대상 | 내용 | 명령 |
|---|---|---|
| 개발 도구 | PC에 설치한 `backend` | `backend update` |
| 프로젝트 SDK | 프로젝트 `dll` 폴더의 뒤끝펑션 SDK DLL | `backend update --project <프로젝트 폴더> --yes` |

개발 도구를 업데이트해도 프로젝트의 SDK DLL은 자동으로 바뀌지 않습니다.  
새 SDK의 기능이나 수정 사항을 사용하려면 프로젝트 SDK도 업데이트해 주세요.  

## 개발 도구 업데이트하기

PowerShell 또는 터미널에서 아래 명령을 실행합니다.  
`backend`는 뒤끝이 서명해 둔 최신 버전 정보를 확인하고, 새 버전이 있으면 확인 질문을 표시합니다. 질문이 표시되면 `y`를 입력합니다.  

```powershell
backend update
```

새 버전이 있으면 아래와 같이 표시됩니다. 버전 번호와 설치 폴더는 환경에 따라 다릅니다.  

```text
Checking https://developer.thebackend.io/sdk/BackendFunction/live/latest.json
Installed  1.1.0
Available  1.1.1   (source b9a9702, SDK b1a74ed)
Update BACKND CLI 1.1.0 -> 1.1.1 for win-x64 into C:\Users\<사용자 이름>\AppData\Local\AFI\Function? [y/N]:
Downloading backend-1.1.1-win-x64.zip ...
Archive verified.
Activated 1.1.1. Previous versions are kept under C:\Users\<사용자 이름>\AppData\Local\AFI\Function\versions.
```

Mac에서는 `win-x64` 대신 `osx-arm64`가, 설치 폴더는 `~/Library/Application Support/AFI/Function`이 표시됩니다.  
이미 최신 버전이면 질문 없이 `Already up to date (1.1.1).`을 표시하고 아무것도 바꾸지 않습니다.  

- 새 버전을 별도 폴더에 설치한 뒤 전환하므로, 업데이트 중에 실패해도 기존 버전을 계속 사용할 수 있습니다.  
- 내려받은 파일은 서명과 게시자 확인을 모두 통과해야 설치됩니다. 통과하지 못하면 아무것도 바꾸지 않습니다.  
- 등록한 인증 정보(`profiles`)와 프로젝트는 그대로 유지됩니다.  
- `backend update`는 옛 버전으로 되돌리지 않습니다.  

## 프로젝트 SDK 업데이트하기

### 1. 바뀔 내용 확인하기

프로젝트 폴더에서 아래 명령을 실행합니다.  
`--yes` 없이 실행하면 개발 도구만 업데이트하고(새 버전이 있으면 확인 질문이 나오며 `y`를 입력합니다), 프로젝트는 바꾸지 않고 바뀔 내용만 보여 줍니다.  

```powershell
cd C:\work\MyFunction
backend update --project .
```

결과의 `data.project.preview.files`에서 DLL별로 바뀌는지(`changed`) 확인할 수 있습니다.  

### 2. 적용하기

내용을 확인했다면 `--yes`를 추가해 적용합니다.  

```powershell
backend update --project . --yes
```

결과의 `data.project.applied`가 `true`이면 완료된 것입니다.  

- `dll` 폴더의 SDK DLL, `backend-sdk.json`, `backend-function.json`의 SDK 버전 정보가 바뀝니다.  
- 필요하면 `BackendFunction.csproj`의 `System.Text.Json` 참조 설정도 맞춥니다.  
- 바꾸기 전 파일은 `.backend/sdk-update-backups` 폴더에 백업됩니다. 결과의 `data.project.backup`에 백업 위치가 표시됩니다.  
- `Function.cs` 등 함수 코드와 `requests` 폴더의 요청 파일, 예전 `debugConfig.json`은 바뀌지 않습니다.  

업데이트한 뒤 `backend debug`로 함수가 정상적으로 실행되는지 확인하고 다시 배포해 주세요.  
**서버에 이미 배포된 함수는 다시 배포하기 전까지 이전 SDK로 실행됩니다.**  

## 개발 도구 업데이트에 실패한 경우

`backend update`가 실패하면 `error.code`와 JSON 위에 표시된 메시지를 확인합니다. 어떤 경우에도 기존 버전은 그대로 남아 계속 사용할 수 있습니다.  

| 오류 코드 | 원인과 해결 방법 |
|---|---|
| `NOT_INSTALLED` | 설치 기록이 없습니다. [Windows](/sdk-docs/function/work-in-windows/install) / [Mac](/sdk-docs/function/work-in-ios/install) 문서의 설치 명령을 먼저 실행합니다. |
| `POINTER_NOT_FOUND` | 서버에서 최신 버전 정보를 찾지 못했습니다. 잠시 뒤 다시 실행합니다. |
| `NETWORK_ERROR` | 최신 버전 정보나 업데이트 파일을 내려받지 못했습니다. 인터넷 연결과 방화벽을 확인합니다. 회사 보안 프로그램이 HTTPS 통신을 검사하면 `backend`가 파일을 내려받지 못할 수 있으니 보안 담당자에게 문의해 주세요. |
| `INTEGRITY_ERROR`, `SIGNATURE_INVALID` | 내려받은 파일의 서명이나 내용이 맞지 않아 업데이트하지 않았습니다. 잠시 뒤 다시 실행하고, 계속되면 뒤끝에 문의해 주세요. |
| `PLATFORM_SIGNATURE_INVALID` | 새 버전의 게시자 서명을 확인하지 못했습니다. 오랫동안 업데이트하지 않은 `backend`에서 게시자 서명이 바뀐 경우에도 나타납니다. 설치 명령을 다시 실행해 설치합니다. |
| `TRUST_ROOT_CHANGED`, `RECEIPT_UNSUPPORTED` | 설치된 `backend`가 가진 서명 확인 키로는 새 버전의 서명을 확인할 수 없습니다. 뒤끝이 서명 키 변경을 공지했거나 예전 시험용 설치 기록이 남아 있는 경우에만 나타납니다. 아래 명령으로 설치 기록(`receipt.json`)과 `bin` 폴더만 삭제하고 설치 명령을 다시 실행합니다. 인증 정보(`profiles`)와 프로젝트는 유지됩니다. |

`TRUST_ROOT_CHANGED`, `RECEIPT_UNSUPPORTED`일 때 삭제하는 명령입니다. 기본 설치 폴더 기준입니다.  

**Windows (PowerShell)**

```powershell
Remove-Item "$env:LOCALAPPDATA\AFI\Function\receipt.json"
Remove-Item -Recurse "$env:LOCALAPPDATA\AFI\Function\bin"
```

**Mac (터미널)**

```bash
rm "$HOME/Library/Application Support/AFI/Function/receipt.json"
rm -r "$HOME/Library/Application Support/AFI/Function/bin"
```

## 프로젝트 SDK 업데이트에 실패한 경우

프로젝트 SDK 업데이트에 실패하면 결과의 `error.code`는 항상 `PROJECT_UPDATE_FAILED`입니다.  
개발 도구는 이미 업데이트되었고, 프로젝트는 업데이트 전 상태로 유지됩니다.  
실패한 원인은 JSON 위에 표시된 프로젝트 업데이트 결과의 `error.code`에서 확인합니다.  

| 원인 오류 코드 | 원인과 해결 방법 |
|---|---|
| `PROJECT_MODIFIED` | `dll` 폴더의 DLL이 기록된 버전과 다릅니다. 직접 바꾼 DLL은 덮어쓰지 않습니다. 직접 바꾼 이유를 확인한 뒤, 원래 DLL로 되돌리고 다시 실행합니다. |
| `SDK_DEPENDENCY_CONFLICT` | 프로젝트의 `System.Text.Json` 참조 설정을 자동으로 맞출 수 없습니다. `BackendFunction.csproj`에서 `System.Text.Json` 패키지 참조를 조건 없이 9.x 버전 숫자로 직접 지정하고, `CopyLocalLockFileAssemblies`를 `true`로 설정한 뒤 다시 실행합니다. 프로젝트 폴더 위쪽에 `Directory.Packages.props`가 있거나, 패키지 버전을 중앙에서 관리하거나 `Import`를 포함한 `Directory.Build.props`·`Directory.Build.targets`가 있어도 같은 오류가 납니다. 이 경우 해당 파일의 설정을 정리한 뒤 다시 실행합니다. |
| `PROJECT_RECOVERY_REQUIRED` | 이전 업데이트가 중간에 멈췄습니다. `.backend/sdk-update-pending.json`에 적힌 백업 파일을 원래 위치로 복원하고 이 파일을 삭제한 뒤 다시 실행합니다. |
| `PROJECT_NOT_FOUND` | 지정한 폴더에 `backend-function.json`이 없습니다. 1.0.2 이하 버전 프로젝트라면 [기존 버전에서 옮기기](/sdk-docs/function/migrate-from-old-version)를 먼저 진행합니다. |
