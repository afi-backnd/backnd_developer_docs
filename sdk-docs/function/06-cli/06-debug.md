---
sidebar_label: 로컬 실행
description: "로컬 실행"
---

# 로컬 실행

`backend debug`는 서버에 배포하지 않고 내 PC에서 함수를 실행해 결과를 보여 줍니다.  
중단점을 걸어 코드를 확인하려면 편집기의 디버깅 기능을 사용합니다.  

- [Visual Studio Code (Windows)](/sdk-docs/function/work-in-windows/visual-studio-code/debug)
- [Visual Studio](/sdk-docs/function/work-in-windows/visual-studio/debug)
- [Visual Studio Code (Mac)](/sdk-docs/function/work-in-ios/visual-studio-code/debug)

## 실행하기

프로젝트 폴더에서 아래 명령을 실행합니다. 인증 정보를 등록하지 않아도 실행할 수 있습니다.  

```powershell
backend debug
```

`backend`가 함수를 디버깅용으로 빌드하고, `requests/default.json`의 내용을 함수에 전달해 실행합니다.  
`execution` 항목에서 실행 결과를 확인합니다.  

```json
{
  "command": "debug",
  "data": {
    "auth": "offline",
    "breakpointDebugging": false,
    "execution": {
      "error": null,
      "ok": true,
      "result": {
        "input": {
          "message": "hello"
        },
        "message": "Hello from Backend Function"
      }
    },
    "mode": "local-execution",
    "profile": null,
    "request": {
      "bytes": 25,
      "path": "requests/default.json",
      "sha256": "388e152ac931fcdf635d0ed094a421401d956295019ec5ae32bb5f91b513e81b"
    },
    "resultFile": "C:\\work\\MyFunction\\.backend\\debug\\result.json",
    "timeoutSeconds": 10,
    "timeoutSource": "default"
  },
  "error": null,
  "ok": true,
  "schemaVersion": 1
}
```

| 항목 | 설명 |
|---|---|
| `execution.ok` | `true`면 함수가 예외 없이 끝났다는 뜻입니다. |
| `execution.result` | 함수가 반환한 값입니다. JSON이면 JSON으로, 그 밖의 문자열이면 문자열로 표시됩니다. |
| `auth` | `offline`은 인증 정보 없이 실행했다는 뜻입니다. `--user`로 실행하면 `test-user`, 예전 `debugConfig.json`으로 실행하면 `legacy`입니다. |
| `request` | 함수에 전달한 요청 파일의 경로와 크기입니다. |
| `timeoutSeconds`, `timeoutSource` | 적용된 제한 시간(초)과 그 출처(`default`, `flag`, `legacy`)입니다. |
| `resultFile` | 같은 결과가 저장되는 파일입니다. |

`execution.ok`는 함수 실행 성공 여부입니다. 함수가 `{"error": ...}`처럼 실패를 뜻하는 값을 반환해도 실행 자체가 끝났다면 `true`입니다. 반환 값은 `result`에서 확인해 주세요.  

:::info 기본 실행은 오프라인입니다
`backend debug`는 인증 정보를 함수에 전달하지 않습니다. 함수가 뒤끝 기능을 호출해도 인증 정보가 없으므로 실제 데이터가 바뀌지 않습니다.  
함수 안에서 게임 정보 저장 같은 뒤끝 기능을 호출하는 경우에는 아래 [테스트 계정으로 실행하기](#테스트-계정으로-실행하기)를 사용합니다.  
:::

## 요청 파일

함수에 전달할 값은 프로젝트의 `requests` 폴더에 JSON 파일로 둡니다.  
`backend new`로 프로젝트를 만들면 `requests/default.json`이 함께 만들어지고, `--data`를 지정하지 않으면 이 파일을 사용합니다.  

요청 파일은 유니티에서 `Backend.BFunc.InvokeFunction`을 호출할 때 `Param`으로 보내는 값과 같은 형태의 JSON 객체(`{ }`)입니다.  
예를 들어 유니티에서 아래와 같이 호출한다고 가정합니다.  

```csharp
Param param = new Param();
param.Add("value", "HelloWorld!");
Backend.BFunc.InvokeFunction("MyFunction", param);
```

이때 요청 파일은 아래와 같습니다.  

```json
{
  "value": "HelloWorld!"
}
```

### 여러 입력으로 실행하기

입력 값을 바꿔 가며 실행하려면 `requests` 폴더에 파일을 더 만들고 `--data`로 지정합니다.  
예를 들어 `requests/second.json`을 만들었다면 아래와 같이 실행합니다.  

```powershell
backend debug --data second
```

`--data`에는 두 가지 형태를 쓸 수 있습니다.  

| 입력 | 의미 |
|---|---|
| `second` (영문, 숫자, `-`, `_`로 된 이름. 최대 64자) | 프로젝트의 `requests/second.json`을 사용합니다. |
| `..\inputs\case1.json`처럼 `.`, `\`, `/`가 들어간 값 | 파일 경로로 처리합니다. **프로젝트 폴더가 아니라 명령을 실행한 현재 폴더 기준**입니다. |

- `--data second.json`처럼 이름에 `.json`을 붙이면 이름이 아니라 경로로 처리되어 현재 폴더의 `second.json`을 찾습니다. `requests` 폴더의 파일은 `.json` 없이 이름만 적어 주세요.  
- 파일이 없으면 `REQUEST_MISSING` 오류가 납니다.  

  ```json
  "error": {
    "code": "REQUEST_MISSING",
    "message": "Request file requests/nothere.json does not exist"
  }
  ```

- 요청 파일은 JSON 객체여야 합니다. `[1, 2]`처럼 배열이나 숫자를 넣으면 `INVALID_ARGUMENT` 오류(`The request must be a JSON object, matching the SDK Param`)가 납니다.  
- 같은 키를 두 번 적으면 오류가 나고, 파일 크기는 4MB까지입니다. 인코딩은 UTF-8입니다.  
- `backend debug`는 `--data -`(입력에서 읽기)를 지원하지 않습니다.  

:::caution 요청 파일에는 인증 정보를 적지 마세요
`requests` 폴더의 파일은 `.gitignore`에 포함되지 않아 **Git에 올라갑니다.**  
인증키, 시그니처 키, 액세스 토큰, 비밀번호는 요청 파일에 적지 마세요. 테스트 계정으로 실행할 때 요청 파일에 등록된 인증 정보와 같은 값이 들어 있으면 실행하지 않고 `REQUEST_CONTAINS_CREDENTIAL` 오류로 끝납니다.  
:::

## 테스트 계정으로 실행하기

함수 안에서 `Backend.Initialize` 뒤에 게임 정보 저장, 영수증 검증 같은 뒤끝 기능을 호출하려면 테스트 계정으로 로그인한 상태로 실행해야 합니다.  
`--user`를 추가하면 [인증 정보 등록](/sdk-docs/function/cli/setting)에서 로그인해 둔 테스트 계정으로 실행합니다.  

**준비:** `backend config`로 뒤끝펑션 인증키, 클라이언트 앱 ID, 시그니처 키, 패키지 이름을 등록하고 테스트 계정에 로그인해 두어야 합니다. 함수 안에서 호출하는 뒤끝 기능은 인증키가 없으면 인증 오류(`BadUnauthorizedException`)로 실패합니다. 멀티 캐릭터 프로젝트는 로그인할 때 고른 캐릭터로 실행됩니다.  

```powershell
backend debug --user
```

실행하면 아래 안내가 표시됩니다. 프로필 이름은 사용하는 프로필에 따라 다릅니다.  

```text
Running as test user (profile 'default'): the function's BACKND calls reach the live server and can change data.
[backend] Test-user session: profile 'default' from the profile store
```

결과의 `auth`가 `test-user`이고 `profile`에 사용한 프로필 이름이 표시됩니다.  

:::caution 실제 데이터가 바뀝니다
`--user`로 실행하면 함수가 호출하는 뒤끝 기능이 실제 서버에서 실행됩니다.  
테스트 계정과 테스트용 프로젝트에서 실행해 주세요.  
테스트 계정 로그인 정보와 인증키가 함수에 전달되므로, 직접 작성했거나 내용을 확인한 코드만 실행해 주세요. Mac에서는 같은 사용자 계정으로 실행되는 다른 프로그램도 이 로그인 정보를 받을 수 있습니다.  
:::

- 로그인 정보가 없으면 실행하기 전에 `AUTH_REQUIRED` 오류로 끝납니다. `backend config`로 로그인해 주세요.  

  ```json
  "error": {
    "code": "AUTH_REQUIRED",
    "message": "No test-user session for profile 'default'. Run backend config and log in a test user."
  }
  ```

- 로그인 정보가 이미 만료로 표시되어 있으면 실행하기 전에 `AUTH_SESSION_EXPIRED` 오류로 끝납니다. `backend config`로 다시 로그인해 주세요. 그 밖의 경우에는 실행하는 동안 필요하면 자동으로 갱신합니다.  
- 사용하는 프로필은 `--profile`, 프로젝트의 `authProfile`, `default` 순서로 정해집니다.  
- 로그인 정보를 가져오는 시간은 함수의 제한 시간에 포함되지 않습니다.  

## 시간 제한

`backend debug`는 서버와 같은 **10초** 제한으로 실행합니다. 10초를 넘으면 함수를 중단합니다.  
제한 시간을 바꾸려면 `--timeout`으로 초 단위를 지정합니다. `0`이면 제한이 없습니다.  

```powershell
backend debug --timeout 20
backend debug --timeout 0
```

제한 시간을 넘으면 결과가 `FUNCTION_EXECUTION_FAILED`로 끝나고, JSON 위에 아래 메시지가 표시됩니다.  

```text
EXECUTION_CANCELLED: Function execution was cancelled or exceeded its timeout.
```

JSON의 `error.details`에는 `"hostErrorCode": "EXECUTION_CANCELLED"`가 표시됩니다.  
자세한 내용은 [타임아웃](/sdk-docs/function/basic/timeout) 문서를 참고해 주세요.  

## 빌드만 하기

`--prepare`는 함수를 실행하지 않고 디버깅에 필요한 파일만 준비합니다.  
Visual Studio Code의 F5 디버깅이 실행 전에 이 명령을 사용합니다. Visual Studio Code의 테스트 계정 F5는 `--prepare --user`를 사용해, 실행하기 전에 로그인 정보가 있는지 확인합니다.  
Visual Studio에서 실행할 때 `BackendFunctionHost.exe`를 찾을 수 없다는 오류가 나면 이 명령을 한 번 실행합니다.  

```powershell
backend debug --prepare
```

## 예전 debugConfig.json으로 실행하기

1.0.2 이하 버전에서 만든 프로젝트의 `debugConfig.json`은 `backend debug`가 **기본으로 읽지 않습니다.** 파일이 있으면 아래 안내가 표시되고, `requests/default.json`을 사용합니다.  

```text
debugConfig.json is ignored; it is only used with --config. Run 'backend migrate' to convert it.
```

`backend migrate`로 `debugConfig.json`의 `content`를 `requests/default.json`으로 옮기는 방법은 [기존 버전에서 옮기기](/sdk-docs/function/migrate-from-old-version#6-요청-파일-만들기)를 참고해 주세요.  
옮기기 전에 예전 방식 그대로 실행하려면 `--config`로 파일을 직접 지정합니다. 경로는 프로젝트 폴더 기준입니다.  

```powershell
backend debug --config debugConfig.json
```

`--config`는 `--data`, `--user`와 함께 사용할 수 없습니다. 실행하면 `debugConfig.json is the legacy debug configuration. ...` 안내가 표시되며, 결과의 `auth`는 `legacy`입니다.  

| 항목 | 필수 | 설명 |
|---|---|---|
| `mode` | | `offline`: 로그인 없이 함수만 실행합니다.<br/>`authenticated`: 테스트 계정으로 로그인한 뒤 실행합니다.<br/>**생략하면 `authenticated`입니다.** |
| `content` | 필수 | 함수에 전달할 값입니다. |
| `timeoutEnable` | | `true`: 5초가 지나면 실행을 중단합니다.<br/>`false`: 시간 제한이 없습니다.<br/>생략하면 10초입니다. 빈 값(`""`)은 `CONFIG_INVALID` 오류입니다. |
| `clientAppId`, `signatureKey`, `packageName`, `authKey` | `authenticated`일 때 | 뒤끝 콘솔의 값입니다. |
| `testId`, `testPw` | `authenticated`일 때 | 로그인할 테스트 계정의 ID와 비밀번호입니다. |
| `accessToken` | | 입력하면 `testId`, `testPw`로 로그인하지 않고 이 토큰을 사용합니다. |

- `mode`가 없는 파일은 테스트 계정으로 로그인해 실제 서버에서 실행하므로 **실제 데이터가 바뀝니다.** 로그인 없이 실행하려면 `"mode": "offline"`을 추가합니다.  
- 환경 변수 `BACKEND_CLIENT_APP_ID`, `BACKEND_SIGNATURE_KEY`, `BACKEND_PACKAGE_NAME`, `BACKEND_AUTH_KEY`, `BACKEND_ACCESS_TOKEN`, `BACKEND_TEST_ID`, `BACKEND_TEST_PW`가 있으면 파일의 같은 항목보다 우선합니다.  
- 예전 방식은 새 프로젝트에서는 사용하지 않습니다. 인증 정보는 `debugConfig.json`에 적지 말고 [`backend config`](/sdk-docs/function/cli/setting#기존-debugconfigjson에서-가져오기)로 등록해 주세요.  

## 내 PC 실행과 서버 실행의 차이

- 내 PC에서는 디버깅용으로 빌드한 함수를 내 운영체제에서 실행합니다. 서버에서는 서버용(`linux-x64`)으로 빌드한 함수를 실행합니다.  
- 함수가 받는 `ILambdaContext`는 내 PC에서 임의로 만든 값입니다.  
- 함수 안에서 호출하는 뒤끝 기능의 권한과 응답 시간이 서버와 다를 수 있습니다.  

내 PC에서 성공했더라도 배포한 뒤 유니티에서 실제로 호출해 결과를 확인해 주세요.  

## 오류 코드

`backend debug`가 실패하면 `error.code`와 JSON 위에 표시된 메시지를 확인합니다.  

| 오류 코드 | 원인과 해결 방법 |
|---|---|
| `REQUEST_MISSING` | 요청 파일이 없습니다. `requests/default.json`을 만들거나 `--data`로 파일을 지정합니다. 예전 `debugConfig.json`만 있다면 `backend migrate`를 실행하거나 `--config`로 지정합니다. |
| `INVALID_ARGUMENT` | 요청 파일이 JSON 객체가 아니거나, 같은 키가 반복되거나, 크기를 넘었습니다. 메시지를 확인합니다. |
| `AUTH_REQUIRED`, `AUTH_SESSION_EXPIRED` | `--user`로 실행했는데 테스트 계정 로그인 정보가 없거나 만료되었습니다. [인증 정보 등록](/sdk-docs/function/cli/setting)에서 `backend config`를 실행해 로그인합니다. |
| `BUILD_FAILED` | 함수 코드에 컴파일 오류가 있습니다. [빌드](/sdk-docs/function/cli/build#빌드에-실패한-경우) 문서를 참고해 주세요. |
| `HOST_BUILD_FAILED` | 실행에 필요한 도구를 빌드하지 못했습니다. `backend doctor`로 .NET 준비 상태를 확인합니다. |
| `CONFIG_MISSING` | `--config`로 지정한 파일이 없습니다. |
| `FUNCTION_EXECUTION_FAILED` | 함수 실행 중 오류가 발생했습니다. JSON 위에 표시된 오류 코드와 `error.details.hostErrorCode`를 아래 표에서 확인합니다. |
| `HOST_FAILED` | 함수를 실행하는 프로그램이 결과를 남기지 않고 끝났습니다. 함수 코드가 프로세스를 갑자기 종료시키지 않는지(예: `Environment.Exit` 호출) 확인하고, 편집기에서 중단점을 걸어 확인합니다. |
| `PROJECT_NOT_FOUND`, `INVALID_PROJECT`, `MIGRATION_REQUIRED`, `BUSY` | 프로젝트를 찾거나 빌드하는 단계의 오류입니다. [빌드](/sdk-docs/function/cli/build#빌드에-실패한-경우) 문서의 오류 코드 표를 참고해 주세요. |

함수 실행 오류의 세부 코드입니다. `.backend/debug/result.json`의 `error.code`에도 같은 값이 저장됩니다.  

| 세부 오류 코드 | 원인과 해결 방법 |
|---|---|
| `REQUEST_UNREADABLE` | 요청 파일을 읽지 못했습니다. 파일 경로와 권한을 확인합니다. |
| `REQUEST_INVALID` | 요청 파일이 올바른 JSON 객체가 아니거나, 같은 키가 반복되거나, 4MB를 넘습니다. |
| `REQUEST_CONTAINS_CREDENTIAL` | 요청 파일에 등록된 인증 정보와 같은 값이 들어 있습니다. 파일에서 지워 주세요. |
| `BROKER_FAILED`, `BROKER_TIMEOUT`, `BROKER_UNAVAILABLE`, `BROKER_RESPONSE_INVALID` | `--user` 또는 테스트 계정 F5에서 로그인 정보를 가져오지 못했습니다. 메시지의 `code`(예: `AUTH_REQUIRED`)를 확인하고 `backend config`로 로그인합니다. `BROKER_TIMEOUT`은 45초 안에 끝나지 않은 경우입니다. |
| `EXECUTION_CANCELLED` | 시간 제한을 넘었습니다. [타임아웃](/sdk-docs/function/basic/timeout) 문서를 참고해 주세요. |
| `HANDLER_INVALID` | `BFunc` 클래스의 `Function` 함수 형태가 맞지 않습니다. [서버에서 뒤끝펑션이 호출되는 형태](/sdk-docs/function/basic/call) 문서를 참고해 주세요. |
| `ASSEMBLY_LOAD_FAILED` | 함수 DLL이나 함께 사용하는 DLL을 불러오지 못했습니다. `dll` 폴더와 프로젝트 참조를 확인합니다. |
| `FUNCTION_FAILED` | 함수 코드에서 처리하지 않은 예외가 발생했습니다. 편집기에서 중단점을 걸고 예외 위치를 확인합니다. 예외 메시지에는 인증 정보가 들어갈 수 있어 표시하지 않습니다. |
| `RESULT_INVALID` | 함수가 읽을 수 없는 Stream을 반환했거나 UTF-8 문자열이 아닌 값을 반환했습니다. |
| `RESULT_TOO_LARGE` | 반환 값이 16MB를 넘습니다. |
| `CONFIG_INVALID`, `CONFIG_UNREADABLE`, `AUTH_CONFIG_INCOMPLETE`, `AUTH_LOGIN_FAILED`, `AUTH_NETWORK_ERROR`, `AUTH_TIMEOUT` | `--config`로 예전 `debugConfig.json`을 실행할 때만 나옵니다. JSON 문법(주석 `//`은 쓸 수 없습니다), `content`, `mode`, `timeoutEnable` 값과 로그인에 필요한 항목, 테스트 계정 정보, 인터넷 연결을 확인합니다. |
