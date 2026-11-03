---
sidebar_label: 디버깅
description: "디버깅"
---

# Visual Studio에서 디버깅하기

서버에 배포하기 전에 내 PC에서 함수를 실행하고 중단점으로 코드를 확인할 수 있습니다.  
[프로젝트 만들기](/sdk-docs/function/work-in-windows/visual-studio/create)를 먼저 완료해 주세요.  

## 1. 함수에 전달할 값 정하기

프로젝트 폴더의 `requests/default.json`을 엽니다.  
이 파일의 내용이 함수에 전달됩니다. 유니티에서 `Backend.BFunc.InvokeFunction`을 호출할 때 `Param`으로 보내는 값과 같은 형태로 작성합니다.  

```json
{
  "message": "hello"
}
```

- `requests` 폴더의 파일은 `.gitignore`에 포함되지 않아 **Git에 올라갑니다.** 인증키, 시그니처 키, 비밀번호는 적지 마세요.  
- 다른 파일로 실행하고 싶다면 `requests` 폴더에 파일을 만들고 `Properties/launchSettings.json`에서 `--request` 다음 경로를 그 파일로 바꿉니다.  

## 2. 실행 프로필 고르기

프로젝트에는 실행 프로필이 두 가지 만들어져 있습니다. 상단 도구 모음의 시작 버튼 옆 목록에서 고릅니다.  

| 실행 프로필 | 설명 |
|---|---|
| **Backend Function (offline)** | 인증 정보 없이 함수만 실행합니다. 함수가 뒤끝 기능을 호출해도 인증 정보가 없으므로 데이터가 바뀌지 않습니다. 처음에는 이 프로필로 실행합니다. |
| **Backend Function (test user)** | 등록해 둔 테스트 계정으로 실행합니다. 함수 안에서 게임 정보 저장 같은 뒤끝 기능을 호출할 때 사용합니다. 실제 서버에서 실행되며, 사용하기 전에 [준비](#테스트-계정으로-실행하기)가 필요합니다. |

## 3. 중단점을 설정하고 실행하기

1. `Function.cs`에서 멈추고 싶은 줄의 왼쪽 여백을 클릭해 중단점을 설정합니다.  
2. 상단 도구 모음에서 빌드 구성이 **Debug**인지 확인합니다.  
3. 실행 프로필이 **Backend Function (offline)**인지 확인합니다.  
4. **F5** 키를 누르거나 **디버그 > 디버깅 시작**을 선택합니다.  

Visual Studio가 프로젝트를 빌드한 뒤 함수를 실행하고, 중단점에서 멈춥니다.  
변수 값 확인, 단계별 실행 등 일반 C# 프로그램과 같은 방법으로 디버깅할 수 있습니다.  

## 4. 실행 결과 확인하기

함수가 실행되는 **콘솔 창**에 결과가 표시됩니다. 중단점에서 멈춰 있는 동안에는 표시되지 않고, 계속 실행해 함수가 끝나면 표시됩니다.  
기본 템플릿을 실행하면 아래와 같이 표시됩니다.  

```text
[backend] Function finished: ok (133 ms)
[backend] Result:
{
  "message": "Hello from Backend Function",
  "input": {
    "message": "hello"
  }
}
```

- `ok`는 함수가 정상적으로 끝났다는 뜻이고, `Result:` 아래가 함수의 반환 값입니다.  
- 괄호 안은 실행하는 데 걸린 시간입니다. 중단점에서 멈춘 시간도 포함됩니다.  
- 반환 값이 64KB보다 크면 앞부분만 표시하고, 나머지는 파일에서 확인하라는 안내가 표시됩니다.  
- 함수가 `Console.WriteLine`으로 출력한 내용도 같은 콘솔 창에 표시됩니다.  

함수가 실패하면 `Function failed:`와 오류 코드가 표시됩니다. 아래는 요청 파일을 읽지 못했을 때의 예시입니다.  

```text
REQUEST_UNREADABLE: The request file could not be read.
[backend] Function failed: REQUEST_UNREADABLE (0 ms)
[backend] The request file could not be read.
```

Visual Studio의 실행 프로필은 결과를 읽을 수 있도록 마지막에 `[backend] Press Enter to close this window.`를 표시하고 기다립니다. 결과를 확인한 뒤 콘솔 창에서 Enter를 누르면 창이 닫힙니다.  

같은 결과는 프로젝트 폴더의 `.backend\debug\result.json` 파일에도 저장됩니다.  
PowerShell에서 프로젝트 폴더로 이동한 뒤 아래 명령으로 확인할 수 있습니다.  

```powershell
Get-Content .backend\debug\result.json
```

기본 템플릿을 실행하면 아래와 같이 저장됩니다.  
`"ok": true`는 함수가 정상적으로 실행되었다는 뜻이고, `result`가 함수의 반환 값입니다.  

```json
{"ok":true,"result":{"message":"Hello from Backend Function","input":{"message":"hello"}},"error":null}
```

:::tip 중단점 없이 결과만 확인하기
PowerShell에서 프로젝트 폴더로 이동한 뒤 `backend debug`를 실행하면 빌드와 실행을 한 번에 하고 결과를 바로 보여 줍니다.  
자세한 내용은 [로컬 실행](/sdk-docs/function/cli/debug) 문서를 참고해 주세요.  
:::

## 테스트 계정으로 실행하기

함수 안에서 `Backend.Initialize` 뒤에 게임 정보, 영수증 검증 같은 뒤끝 기능을 호출하려면 테스트 계정으로 로그인한 상태로 실행해야 합니다.  

### 준비

프로젝트 폴더에서 PowerShell을 열고 아래 명령을 실행해 인증 정보를 등록하고 테스트 계정에 로그인해 둡니다. 한 번만 하면 됩니다.  

```powershell
backend config
```

뒤끝펑션 인증키, 클라이언트 앱 ID, 시그니처 키, 패키지 이름을 입력하고, `Log in a test user ...? [y/N]` 질문에 `y`를 입력해 테스트 계정 ID와 비밀번호로 로그인합니다. 인증키가 없으면 함수 안에서 호출하는 뒤끝 기능이 인증 오류로 실패합니다. 자세한 방법은 [인증 정보 등록](/sdk-docs/function/cli/setting)을 참고해 주세요.  
멀티 캐릭터 프로젝트는 로그인할 때 고른 캐릭터로 실행됩니다.  

### 실행

1. 실행 프로필을 **Backend Function (test user)**로 바꿉니다.  
2. **F5** 키를 누릅니다.  

콘솔 창에 아래 안내가 먼저 표시되고, 이어서 함수의 실행 결과가 표시됩니다.  

```text
[backend] Test-user session: profile 'default' from the profile store
```

멀티 캐릭터 프로젝트라면 `, character 'Knight'`처럼 캐릭터 이름이 함께 표시됩니다. 사용하는 프로필은 프로젝트의 `authProfile`, 없으면 `default`입니다.  

:::caution 실제 데이터가 바뀝니다
테스트 계정 프로필로 실행하면 함수가 호출하는 뒤끝 기능이 실제 서버에서 실행됩니다.  
테스트 계정과 테스트용 프로젝트에서 실행해 주세요.  
테스트 계정 로그인 정보와 인증키가 함수에 전달되므로, 직접 작성했거나 내용을 확인한 코드만 실행해 주세요.  
`requests` 폴더의 파일에 등록된 인증 정보와 같은 값이 들어 있으면 함수를 실행하지 않고 `REQUEST_CONTAINS_CREDENTIAL` 오류로 끝납니다.  
:::

## 문제 해결

| 증상 | 확인할 내용 |
|---|---|
| 실행하자마자 `BackendFunctionHost.exe`를 찾을 수 없다는 오류가 납니다. | 프로젝트를 새로 받았거나 `.backend` 폴더를 지운 경우입니다. 프로젝트 폴더에서 `backend debug --prepare`를 한 번 실행한 뒤 다시 실행합니다. |
| **Backend Function (test user)**를 실행했더니 콘솔 창에 `BROKER_FAILED: The credential broker failed with code ...`가 표시됩니다. | 메시지 끝의 코드를 확인합니다. `AUTH_REQUIRED`(로그인 정보 없음), `AUTH_SESSION_EXPIRED`(만료)는 위 [준비](#준비)의 `backend config`로 다시 로그인하고, `AUTH_DEVICE_BLOCKED`는 뒤끝 콘솔에서 차단 여부를 확인합니다. |
| `BROKER_UNAVAILABLE`이 표시됩니다. | `backend`를 다시 설치하거나 이동해 실행 경로가 바뀐 경우입니다. 프로젝트 폴더에서 `backend debug --prepare`를 한 번 실행한 뒤 다시 실행합니다. |
| 콘솔 창 또는 `.backend\debug\result.json`에 `REQUEST_UNREADABLE`이 표시됩니다. | `requests\default.json` 파일이 없습니다. 파일을 만들거나, 프로젝트를 새로 받았다면 파일이 Git에 올라와 있는지 확인합니다. 내용은 `{}`처럼 JSON 객체여야 합니다. |
| `REQUEST_INVALID`가 표시됩니다. | 요청 파일이 올바른 JSON이 아닙니다. 쉼표, 따옴표, `//` 주석을 확인합니다. |
| `HANDLER_INVALID`가 표시됩니다. | `BFunc` 클래스의 `Function` 함수 형태를 확인합니다. [서버에서 뒤끝펑션이 호출되는 형태](/sdk-docs/function/basic/call) 문서를 참고해 주세요. |
| `FUNCTION_FAILED`가 표시됩니다. | 함수 코드에서 처리하지 않은 예외가 발생했습니다. 중단점을 걸고 예외가 발생하는 위치를 확인합니다. |

그 밖의 오류는 [로컬 실행](/sdk-docs/function/cli/debug#오류-코드) 문서를 참고해 주세요.  
