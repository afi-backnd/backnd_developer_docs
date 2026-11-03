---
sidebar_label: Backend CLI
description: "Backend CLI"
---

# Backend CLI

`backend`는 뒤끝펑션 프로젝트 생성, 빌드, 로컬 실행, 배포를 모두 처리하는 명령줄 도구입니다.  
Windows에서는 PowerShell, Mac에서는 터미널에서 사용합니다.  
설치 방법은 [Windows](/sdk-docs/function/work-in-windows/install) / [Mac](/sdk-docs/function/work-in-ios/install) 문서를 참고해 주세요.  

## 사용 형식

```text
backend <명령> [인자] [옵션]
```

명령별 사용법은 `--help`로 확인할 수 있습니다.  

```powershell
backend --help
backend deploy --help
```

## 명령 목록

| 명령 | 설명 | 문서 |
|---|---|---|
| `new` | 새 프로젝트를 만듭니다. | Windows: [Visual Studio Code](/sdk-docs/function/work-in-windows/visual-studio-code/create) / [Visual Studio](/sdk-docs/function/work-in-windows/visual-studio/create), Mac: [Visual Studio Code](/sdk-docs/function/work-in-ios/visual-studio-code/create) |
| `debug` | 내 PC에서 함수를 실행합니다. 기본은 인증 정보 없이(오프라인) 실행하고, `--user`를 붙이면 테스트 계정으로 실행합니다. | [로컬 실행](/sdk-docs/function/cli/debug) |
| `build` | 서버에 배포할 파일을 만듭니다. | [빌드](/sdk-docs/function/cli/build) |
| `config` | 뒤끝펑션 인증키와 테스트 계정 같은 인증 정보를 질문에 답하며 등록하고 확인합니다. `config use`로 프로젝트에 사용할 프로필을 연결합니다. | [인증 정보 등록](/sdk-docs/function/cli/setting) |
| `deploy` | 함수를 빌드해 서버에 배포합니다. | [배포](/sdk-docs/function/cli/deploy) |
| `delete` | 서버에 배포된 함수를 삭제합니다. | [삭제](/sdk-docs/function/cli/delete) |
| `list` | 서버에 배포된 함수 이름 목록을 확인합니다. | [원격 호출과 조회](/sdk-docs/function/cli/invoke) |
| `status` | 함수가 서버에 배포되어 있는지 확인합니다. | [원격 호출과 조회](/sdk-docs/function/cli/invoke) |
| `invoke` | 서버에 배포된 함수를 호출합니다. | [원격 호출과 조회](/sdk-docs/function/cli/invoke) |
| `update` | `backend`와 프로젝트 SDK를 업데이트합니다. | [업데이트](/sdk-docs/function/upgrade) |
| `migrate` | 1.0.2 이하 버전 프로젝트를 1.1.0 형식으로 옮깁니다. | [기존 버전에서 옮기기](/sdk-docs/function/migrate-from-old-version) |
| `doctor` | 설치 상태와 .NET 준비 상태를 점검합니다. | [Windows](/sdk-docs/function/work-in-windows/install#2-설치-확인하기) / [Mac](/sdk-docs/function/work-in-ios/install#2-설치-확인하기) |

## 대상 프로젝트

프로젝트를 다루는 명령(`debug`, `build`, `deploy` 등)은 **현재 폴더에서 시작해 상위 폴더로 올라가며** `backend-function.json` 파일이 있는 프로젝트를 찾습니다.  
보통은 프로젝트 폴더로 이동한 뒤 명령을 실행하면 됩니다.  

```powershell
cd C:\work\MyFunction
backend debug
```

다른 폴더에서 실행하려면 `--project`로 프로젝트 폴더를 지정합니다.  

```powershell
backend debug --project C:\work\MyFunction
```

`backend`는 마지막으로 사용한 프로젝트를 기억하지 않습니다.  

## 사용하는 인증 정보(프로필)

서버에 요청하거나 테스트 계정이 필요한 명령은 **프로필**에 저장된 인증 정보를 사용합니다. 프로필은 `--profile`, 프로젝트의 `authProfile`(`backend config use`로 연결), `default` 순서로 정해집니다. 프로필을 만들고 고르는 방법은 [인증 정보 등록](/sdk-docs/function/cli/setting#여러-프로필-사용하기) 문서를 참고해 주세요.  

서버로 요청하는 명령(`deploy`, `delete`, `invoke`, `list`, `status`)은 실행할 때마다 사용하는 프로필을 **JSON 위에** `Target:` 줄로 표시합니다. 다른 뒤끝 프로젝트에 요청하지 않도록 이 줄을 확인해 주세요.  

```text
Target: profile 'default' (default) -> https://function.uploader.thebackend.io/function/dotnet8
```

## 실행 결과 읽기

명령이 끝나면 결과를 아래 형식의 JSON으로 출력합니다.  

```json
{
  "command": "build",
  "data": { },
  "error": null,
  "ok": true,
  "schemaVersion": 1
}
```

| 항목 | 설명 |
|---|---|
| `ok` | `true`면 성공, `false`면 실패입니다. |
| `data` | 성공했을 때의 결과입니다. 명령마다 내용이 다릅니다. |
| `error` | 실패했을 때 `code`(오류 코드), `message`(설명), `details`(추가 정보)가 들어갑니다. |

빌드 진행 상황, 컴파일 오류, `Target:` 같은 안내와 질문은 JSON 위에 함께 표시됩니다.  
`ok`가 `false`면 JSON 위에 표시된 메시지와 `error.message`를 함께 확인해 주세요.  

## 확인 질문

서버의 함수나 데이터를 바꿀 수 있는 명령(`deploy`, `delete`, `invoke`)과 `update`는 실행 전에 확인 질문을 표시합니다.  
`y` 또는 `yes`를 입력하면 진행하고, 그 밖의 입력은 취소합니다.  
`deploy`와 `delete`는 질문 바로 위에 사용할 프로필을 다시 안내합니다.  
`backend config`도 저장된 값을 다른 값으로 바꾸거나 `--reset`으로 프로필을 비울 때 확인 질문을 표시합니다.  

```text
Deploy function 'MyFunction' with profile 'dev'
  Profile source: authProfile in C:\work\MyFunction\backend-function.json
  ZIP:            C:\work\MyFunction\.backend\publish.zip
  SHA256:         53d077a3d0a4ee9288166b66c8b3216068c021a5ab83fc9ae01a736296f39dec
  Other profiles: live
  Change the project's profile with "backend config use <profile>", or pass --profile <profile> for one command.
Deploy function 'MyFunction' with profile 'dev'? [y/N]:
```

`backend config`는 값을 하나씩 묻는 **대화형 명령**이며, 터미널에서 직접 실행해야 합니다. 자동화에서는 `--stdin`을 사용합니다.  

## 공통 옵션

| 옵션 | 설명 |
|---|---|
| `--project <경로>` | 대상 프로젝트 폴더 또는 `.csproj` 파일을 지정합니다. |
| `--profile <이름>` | 이번 명령에 사용할 인증 정보 프로필을 지정합니다. 지정하지 않으면 프로젝트의 `authProfile`, 그것도 없으면 `default`를 사용합니다. 프로젝트에 계속 사용할 프로필은 `backend config use <이름>`으로 연결합니다. [인증 정보 등록](/sdk-docs/function/cli/setting#여러-프로필-사용하기) 문서를 참고해 주세요. |
| `--yes` | 확인 질문에 미리 동의합니다. 자동화 스크립트에서 사용합니다. |
| `--non-interactive` | 입력을 기다리지 않습니다. 확인이 필요한데 `--yes`가 없으면 `CONFIRMATION_REQUIRED` 오류로 끝납니다. |
| `--json` | 결과 JSON을 한 줄로 출력합니다. |
| `--home <경로>` | 설치 폴더 대신 다른 폴더를 사용합니다. 일반적으로 사용하지 않습니다. |

## 종료 코드

자동화 스크립트에서 결과를 판단할 때 사용합니다.  

| 종료 코드 | 의미 |
|---|---|
| 0 | 성공했습니다. |
| 1 | 실패했습니다. |
| 2 | 명령, 옵션 또는 입력 값이 잘못되었습니다(`USAGE`, `INVALID_ARGUMENT`). 사용할 프로필을 정할 수 없거나(`PROFILE_AMBIGUOUS`), 요청 파일에 인증 정보가 들어 있거나(`REQUEST_CONTAINS_CREDENTIAL`), 캐릭터를 지정하지 않았거나 찾을 수 없는 경우(`CHARACTER_SELECTION_REQUIRED`, `CHARACTER_NOT_FOUND`)도 해당합니다. |
| 3 | 확인이나 인증 정보가 필요합니다(`CONFIRMATION_REQUIRED`, `AUTH_REQUIRED`, `AUTH_INVALID`). 테스트 계정 로그인에 문제가 있는 경우(`AUTH_LOGIN_FAILED`, `AUTH_SESSION_EXPIRED`, `AUTH_NO_CHARACTER`, `AUTH_DEVICE_BLOCKED`)도 해당합니다. |
| 4 | 파일 무결성이나 서명 확인에 실패했습니다(`INTEGRITY_ERROR`, `SIGNATURE_INVALID`, `UNTRUSTED_SOURCE`, `TRUST_ROOT_CHANGED`, `PLATFORM_SIGNATURE_INVALID`). |
| 130 | 사용자가 취소했습니다(`CANCELLED`). Ctrl+C로 중단했거나 확인 질문에 `y`(또는 `yes`) 외의 값을 입력한 경우입니다. |

## 1.0.2 이하 버전 명령과의 차이

| 1.0.2 이하 | 1.1.0 이상 |
|---|---|
| `backend config`: `config.json`을 편집합니다. | `backend config`: 뒤끝펑션 인증키와 테스트 계정을 질문에 답하며 등록합니다. 프로젝트 정보는 프로젝트 폴더의 `backend-function.json`에 있습니다. |
| `backend build` | `backend build`: 현재 폴더의 프로젝트를 빌드합니다. |
| `backend deploy` | `backend deploy`: 빌드까지 함께 진행합니다. |
| `backend delete <함수 이름>` | `backend delete <함수 이름>`: 확인 질문이 추가되었습니다. |
| BF Debug Tool + `debugConfig.json` | `backend debug` + `requests/default.json`: 기본은 오프라인으로 실행하고, `--user`를 붙이면 테스트 계정으로 실행합니다. |
| `backend help` | `backend --help` |
