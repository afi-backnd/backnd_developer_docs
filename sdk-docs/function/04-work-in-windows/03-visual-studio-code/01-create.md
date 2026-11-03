---
sidebar_label: 프로젝트 생성
description: "프로젝트 생성"
---

# Visual Studio Code 프로젝트 만들기

`backend new` 명령으로 Visual Studio Code용 뒤끝펑션 프로젝트를 만들고 Visual Studio Code에서 엽니다.  
[backend 설치](/sdk-docs/function/work-in-windows/install)를 먼저 완료해 주세요.  

## 1. Visual Studio Code 준비하기

1. [Visual Studio Code](https://code.visualstudio.com/)를 설치합니다.  
2. Visual Studio Code의 **확장**(Ctrl+Shift+X)에서 Microsoft의 **C#** 확장(`ms-dotnettools.csharp`)을 설치합니다.  

:::info BackendFunction 확장은 필요하지 않습니다
1.0.2 이하 버전에서 사용하던 BackendFunction 확장은 1.1.0부터 사용하지 않습니다. 확장 목록에서 제거해도 됩니다.
:::

## 2. 프로젝트 만들기

PowerShell을 열고 프로젝트를 만들 폴더로 이동합니다.  
아래 예시는 `C:\work` 폴더에 `MyFunction` 프로젝트를 만듭니다.  

```powershell
mkdir C:\work -Force
cd C:\work
backend new MyFunction
```

결과에 `"ok": true`가 표시되면 `C:\work\MyFunction` 폴더가 만들어진 것입니다.  

```json
{
  "command": "new",
  "data": {
    "functionName": "MyFunction",
    "ide": "vscode",
    "note": "requests/*.json are committed to Git: never put credentials in a request file. backend debug runs offline by default; add --user to run as the test user.",
    "project": "C:\\work\\MyFunction",
    "requests": [
      "requests/default.json"
    ],
    "sdkVersion": "snapshot-d6ef1b2188e3ac9b",
    "template": "hello"
  },
  "error": null,
  "ok": true,
  "schemaVersion": 1
}
```

`sdkVersion` 값은 설치한 `backend` 버전에 따라 다릅니다.  

:::info 프로젝트 이름 규칙
- 프로젝트 이름은 **서버에 배포될 함수 이름**으로도 사용됩니다.  
- 영문, 숫자, `_`, `-`만 사용할 수 있고 영문 또는 숫자로 시작해야 합니다. 최대 64자입니다. `CON`, `NUL`, `COM1`처럼 Windows가 예약한 이름은 사용할 수 없습니다.  
- 이미 파일이 있는 폴더에는 만들 수 없습니다.  
:::

## 3. Visual Studio Code에서 열기

Visual Studio Code에서 **파일 > 폴더 열기**를 선택하고 `C:\work\MyFunction` 폴더를 엽니다.  
PowerShell에서 열려면 아래와 같이 실행합니다.  

```powershell
code C:\work\MyFunction
```

폴더를 열었을 때 C# 확장 설치를 권장하는 알림이 나오면 설치합니다.  

## 만들어지는 파일

| 파일 | 설명 |
|---|---|
| `Function.cs` | 함수 코드입니다. 이 파일을 수정해 함수를 작성합니다. |
| `BackendFunction.csproj` | 프로젝트 파일입니다. |
| `requests/default.json` | 내 PC에서 실행할 때 함수에 전달할 값입니다. 유니티에서 `InvokeFunction`으로 보내는 `Param`과 같은 형태의 JSON입니다. Git에 올라가는 파일이므로 인증 정보를 적지 않습니다. [디버깅](/sdk-docs/function/work-in-windows/visual-studio-code/debug) 문서를 참고해 주세요. |
| `backend-function.json` | 배포할 함수 이름과 설명 등 프로젝트 정보입니다. |
| `backend-sdk.json` | 프로젝트에 포함된 SDK DLL의 버전 정보입니다. 직접 수정하지 않습니다. |
| `dll/` | 뒤끝펑션 SDK DLL입니다. 직접 수정하지 않습니다. |
| `.vscode/launch.json`, `.vscode/tasks.json` | Visual Studio Code 디버깅 설정입니다. **Backend Function (offline)**(인증 정보 없이 실행)과 **Backend Function (test user)**(등록한 테스트 계정으로 실행) 두 가지 실행 구성과 그 준비 작업이 들어 있습니다. |
| `.vscode/extensions.json` | 권장 확장(C#) 목록입니다. |
| `.backend/` | `backend`가 관리하는 빌드·디버깅용 폴더입니다. 처음 디버깅할 때 만들어집니다. 직접 수정하지 않습니다. |
| `.gitignore` | Git에 올리지 않을 파일 목록입니다. `.backend/`와 예전 버전의 `debugConfig.json`이 포함됩니다. `requests/`는 포함되지 않아 Git에 올라갑니다. |

## 고정되는 이름

서버는 배포된 함수에서 아래 이름을 찾아 실행합니다.  
`backend new`가 만든 프로젝트에는 이미 설정되어 있으므로 바꾸지 마세요.  

- 어셈블리 이름: `BackendFunction` (빌드 결과 `BackendFunction.dll`)
- 네임스페이스: `BackendFunction`
- 클래스: `BFunc`
- 함수: `Function`

프로젝트 폴더 이름을 바꾸어도 위 이름은 바뀌지 않습니다.  
함수 형태는 [서버에서 뒤끝펑션이 호출되는 형태](/sdk-docs/function/basic/call) 문서를 참고해 주세요.  

## 다른 템플릿으로 만들기

기본 템플릿 `hello`는 받은 값을 인사 메시지와 함께 돌려줍니다.  
빈 함수로 시작하려면 `--template empty`를 추가합니다.  

```powershell
backend new MyFunction --template empty
```

두 템플릿의 코드는 [뒤끝펑션 템플릿](/sdk-docs/function/template) 문서를 참고해 주세요.
